# Sizing an AI Test Environment – Part 1: Choose the Hardware

*Estimated reading time: 11 minutes*

Nov 18, 2026 · Russell OConnor

## Design before deciding

If you already have a significant investment in virtualization and good reasons to avoid public cloud lock-in, running AI on-premises may be worth the effort.

This post looks at what that requires from a hardware and software perspective, assuming you are adding GPU-enabled nodes to an existing virtualization platform.

I'm using Nutanix as the baseline platform. Note that you will need software licenses as well as the GPU node itself.

### Server requirements

Plan for four physical servers: the three ordinary Nutanix nodes, and one GPU node that joins the same cluster.

| Server | Quantity | Role in this build | Typical spec for a test |
| --- | --- | --- | --- |
| Existing CPU nodes | 3 (the smallest Nutanix cluster) | Run Prism Central, Nutanix Files and all the Kubernetes VMs that don't need a GPU | Standard AHV nodes, with enough free capacity for the VMs below |
| GPU node | 1 | Runs one GPU worker VM, with the GPU passed through to it | 1 × GPU; 2 × 32-core CPUs; 384–512 GB RAM; 4–6 NVMe drives; 2 × 25 GbE; hardware certified by the vendor for the GPU |

The GPU node runs AOS and AHV like any other node, so it also adds storage and its own Controller VM to the cluster.

### The software, layer by layer

Each Nutanix product does one job. From the bottom of the stack to the top:

| Layer | What it is | What it does in this build | License |
| --- | --- | --- | --- |
| AHV | Nutanix's hypervisor | Runs every VM, and passes the GPU straight through to the GPU worker VM | Part of NCI |
| AOS | Nutanix's distributed storage | Provides the virtual disks for every VM, including the Kubernetes nodes | NCI. Nutanix's [NAI deployment service lists NCI Ultimate as a required license](https://www.nutanix.com/content/dam/nutanix/en/resources/service-descriptions/sd-nai-gpt-deployment.pdf) |
| Prism Central | The management console for one or more clusters | Where you manage the cluster. NKP talks to it to create and remove its VMs | Part of NCI |
| Nutanix Files (part of Nutanix Unified Storage) | File services (NFS and SMB) | Holds the downloaded models on an NFS share, which Kubernetes mounts through a storage driver (CSI) | NUS. [NCI includes 1 TiB of Files/Objects capacity free](https://www.nutanix.com/en_gb/products/cloud-platform/software-options) |
| NKP (Nutanix Kubernetes Platform) | Nutanix's Kubernetes distribution | Builds a Kubernetes cluster out of VMs on AHV, with a load balancer, ingress and a catalog of add-ons | NKP Pro or Ultimate, or the [NAI-NKP bundle](https://www.nutanix.com/uk/products/software-options) |
| NVIDIA GPU Operator | NVIDIA software that runs inside Kubernetes | Installs the GPU driver, lets Kubernetes see and schedule the GPU, and exports GPU metrics | Free; installed from the NKP catalog |
| NAI (Nutanix Enterprise AI) | Nutanix's model-serving platform | Downloads models from Hugging Face or NVIDIA NIM, runs them as OpenAI-compatible endpoints, issues API keys and shows usage and GPU metrics | NAI Pro, [licensed per vCPU or per GB of GPU memory](https://www.nutanix.com/products/software-options) |
| NVIDIA AI Enterprise (optional) | NVIDIA's licensed software suite | Needed for vGPU (sharing a card) and usually for NIM in production. Not needed for plain passthrough with Hugging Face models | NVIDIA license; check with NVIDIA or your reseller |

### Kubernetes platform

NKP builds the Kubernetes platform out of virtual machines. Everything except the GPU worker runs on CPU nodes. NKP's sizing comes from D2iQ, the company whose platform became NKP. Its docs called for [at least three control plane nodes and at least four worker nodes](https://docs.d2iq.com/dkp/2.7/resource-requirements), at [4 vCPU and 16 GB for control plane nodes and 8 vCPU and 32 GB for workers](https://docs.d2iq.com/dkp/2.6/resource-requirements).

Check the current NKP guide before you build.

| VM | Count | vCPU each | RAM each | Runs on |
| --- | --- | --- | --- | --- |
| NKP control plane | 3 | 4 | 16 GB | CPU nodes |
| NKP workers (including NAI's own services) | 4–7 | 8 | 32 GB | CPU nodes |
| GPU worker | 1 | 16 | 192 GB (2 × the 96 GB GPU) | GPU node |
| Prism Central | 1 (a single small VM is enough for a test) | See the Prism Central sizing guide | See the Prism Central sizing guide | CPU nodes |
| Nutanix Files file-server VMs | Per the Files guide (three by default) | See the Files guide | See the Files guide | CPU nodes |

NAI needs three workers for its own services. Plan for up to seven workers in total. However, in a test environment, one NKP cluster may be able to do both jobs: managing Kubernetes and running NAI.

Rough estimate for the Kubernetes VMs: about 60–85 vCPU and 370–470 GB of RAM, plus Prism Central and Files. Check Prism Central's capacity runway if you plan to use existing nodes.

## Start with a conversation, not a spec sheet

Before you price any hardware, sit down with the AI or development team and agree what the test needs to prove. You don't need to speak their language fluently. You need to know enough to ask the right questions, and to spot which answers will drive the hardware bill.

Five questions are enough to start:

1. **What will it do?** For example, a chat assistant over company documents, summarizing reports, etc. Will they only run models, or also train or fine-tune them?
2. **Which model, or what size of model?** They will usually answer with something like "Llama 8B" or "a 70B model".
3. **How many people will use it at the same time** during the test?
4. **What data will it use,** and is that data allowed to leave the data center?
5. **What does success look like,** and when do they need an answer?

I'm not advocating going it alone: it's vital to bring in the right people to validate your choices.

## Choosing the GPU: a GPU 101

For AI, the most important thing about a GPU is how much memory it has and how fast it can read that memory. This section explains the terms you will see on every spec sheet, so you can read one and know what matters.

### First, "GPU" and "card"

A **GPU** (graphics processing unit) is a processor built to do thousands of simple calculations at the same time. AI models are, underneath, huge grids of numbers multiplied together, so this kind of parallel work suits them far better than a server CPU with a few dozen cores.

In a data center server the GPU comes as a **card**: a physical board, about the size of a hardback book, that plugs into a **PCIe slot** on the server's motherboard. It carries the GPU chip, its own memory and its heatsink. Most data center cards are double-width, meaning each one takes up the space of two slots. When this post says "two cards", it means two physical GPUs in one server. A typical 2U server holds one to four cards; larger servers hold eight.

The biggest training systems use a different form, called **SXM**. Here the GPUs are soldered onto a shared board (NVIDIA's HGX) instead of plugging in as cards. For a Nutanix test environment, think PCIe cards.

### The five things to compare

#### 1. VRAM: how much the GPU can hold

**VRAM (video RAM, or GPU memory)** is memory that sits on the card itself, next to the GPU chip. It is separate from the server's normal RAM. How much data the GPU can hold decides which models fit.

Two things must sit in VRAM while a model runs:

- **Model weights.** A model is a large set of numbers learned during training, called **parameters**. "Llama 3.1 8B" means 8 billion parameters. All of them must be loaded into VRAM.
- **KV cache.** The model's short-term memory of each conversation: the prompt, any documents you passed in, and its answer so far. It grows with every word, and with every extra user.

Think of VRAM as the workbench and the server's RAM and disks as the warehouse. The GPU can only work on what is on the bench. If the model does not fit, it either refuses to load, or the overflow is fetched from the warehouse across the slot. That is ten to fifty times slower and makes the test meaningless.

Cards today range from 24 GB (L4) to 141 GB (H200 NVL).

Note on **model weights**  
**Model weights** represent the patterns learned during training. The GPU loads these weights into VRAM and performs **matrix calculations** that produce scores called **logits**. A **logit is a number/value**, while **softmax is a mathematical function** that converts those scores into probabilities, e.g., Paris 84.4%, Lyon 11.4%, London 4.2%.

#### 2. Memory bandwidth: how fast it can read what it holds

**Memory bandwidth** is how many gigabytes per second the GPU can read from its own VRAM. If VRAM is the size of the workbench, bandwidth is how quickly you can pick things up off it.

It matters because of how models write answers. They produce text one **token** at a time (a token is roughly three-quarters of a word). To produce each token, the GPU reads through all of the model's weights. That makes memory speed, measured in GB/s or TB/s, the limit on how fast answers appear.

The type of memory sets the bandwidth:

- **GDDR** (GDDR6, GDDR7) is the same family of memory used in gaming graphics cards. It is cheaper and gives roughly 0.3–1.6 TB/s. The L4, L40S and RTX PRO 6000 use it.
- **HBM** (high-bandwidth memory) is stacked right beside the GPU chip. It is expensive and gives roughly 3–5 TB/s. The H100 and H200 use it.

So two cards with the same VRAM can feel very different to users. HBM cards answer faster per user and hold up better under many users.

#### 3. Precision: how many bytes each number takes

**Precision** is how many bytes are used to store each parameter. Fewer bytes means a smaller model that loads faster and needs less bandwidth, at some cost in accuracy.

Reducing precision is called **quantization**.

| Format | Bytes per parameter | 8B model size | 70B model size | Quality impact |
| --- | --- | --- | --- | --- |
| FP16 / BF16 ("16-bit") | 2 | 16 GB | 140 GB | The usual baseline |
| FP8 / INT8 ("8-bit") | 1 | 8 GB | 70 GB | Usually hard to notice |
| FP4 / INT4 ("4-bit") | 0.5 | 4 GB | 35 GB | Noticeable on some tasks; test it |

A familiar comparison is photo compression. A well-compressed JPEG looks the same as the original at half the size; squeeze it too hard and you start to see the damage.

The catch for hardware buyers: a GPU only gets the full speed benefit from a format it supports natively. Each NVIDIA generation added a smaller format:

- **Ampere** (A100, A10): down to 16-bit and INT8.
- **Ada Lovelace** (L4, L40S) and **Hopper** (H100, H200): add FP8.
- **Blackwell** (RTX PRO 6000, B200): add FP4.

It's important to know whether the AI team plans to run 8-bit or 4-bit models, so you can buy a generation that supports that format.

#### 4. Interconnect: how GPUs talk to the server and to each other

**Interconnect** is the link a GPU uses to move data. There are two kinds to know.

- **PCIe** is the standard slot every card plugs into. It connects the GPU to the server's CPU, RAM and network. A PCIe Gen4 x16 slot moves about 32 GB/s in each direction; Gen5 doubles that to about 64 GB/s.
- **NVLink** is NVIDIA's direct GPU-to-GPU link. On PCIe cards it comes as a small **bridge** clipped across the tops of two or four cards. It runs at hundreds of GB/s, roughly ten times faster than PCIe.

This only matters when one model is too big for one card. Then the inference software splits the model across several GPUs (this is called **tensor parallelism**), and the GPUs swap partial results many times per token. Over NVLink that is fast. Over PCIe it works, but more slowly, and the gap grows with more GPUs.

For a test environment that runs one model per GPU, NVLink is not needed.

#### 5. Sharing a GPU between VMs

On Nutanix AHV there are three ways to give a GPU to virtual machines:

- **Passthrough.** One whole GPU goes to one VM, as if it were plugged into that VM directly. This is the simplest option and gives full performance. It is the usual choice for AI.
- **vGPU.** NVIDIA's software splits a GPU's memory into fixed slices and shares its compute between several VMs. It needs an NVIDIA license. It is good for several small models or a mix of desktops and AI. Nutanix supports [vGPU partitioning of L4, L40 and RTX PRO 6000 Blackwell Server Edition](https://www.nutanix.com/library/solution-briefs/nvidia-vdi-for-ai).
- **MIG (Multi-Instance GPU).** The GPU is split in hardware into fully isolated smaller GPUs, each with its own memory and compute. The RTX PRO 6000 supports [up to four MIG instances](https://www.amax.com/nvidia-rtx-pro-6000-blackwell-server-edition-vs-nvidia-l40s/). It is useful when several teams share one card and must not affect each other.

### Power and cooling: why GPUs come inside certified servers

A data center GPU is not a part you add on your own. It depends on the server around it for power and air, so it is sold and supported as part of a certified (or "qualified") server configuration.

Data center cards are **passively cooled**: they have no fans of their own, and the server's fans push air through the card's heatsink. A server vendor certifies a GPU only after testing a specific combination:

- **Airflow.** Fan count, fan speeds and air baffles that keep the card cool at full load. Without enough air the GPU **throttles**: it slows itself down to avoid damage, so your test results look worse than the hardware can really do.
- **Power.** Power supplies big enough for the CPUs plus every GPU at full draw, with redundancy, and the right auxiliary power cable to each card.
- **Slots and quantity.** Which slots can take a double-width card, and how many cards the chassis supports.
- **Firmware.** BIOS, BMC and fan profiles that recognize the card.

Buying the certified configuration keeps the whole stack supported by the server vendor, Nutanix and NVIDIA.

### Check the rack, not just the server

Every watt a server draws becomes heat that the room must remove. One GPU node can draw 1.2–2.5 kW, as much as four to eight typical virtualization hosts. Many existing enterprise racks were designed for roughly 5–10 kW in total.

Adding GPU nodes to a rack that is already nearly full causes two problems:

- **Power.** The rack's power distribution units (PDUs) and circuits have a fixed limit. With redundant A and B feeds, each feed must carry the whole rack if the other fails, so the usable budget is half of what the PDUs add up to. A GPU node at full load can push the rack over that limit and trip a breaker, taking down everything else in the rack.
- **Cooling.** Cooling is designed per rack or per row. Extra heat from one rack raises the temperature of the air its neighbors draw in. The GPUs throttle, and nearby hosts start raising temperature alarms.

Ask your facilities team three questions: what is the power budget per feed for this rack, what does the rack draw today, and how much heat can the cooling remove at this position?

### Example GPU selection

| GPU | Generation | VRAM | Memory bandwidth (approx.) | Smallest native format | Max power | NVLink | Best fit in a test environment |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NVIDIA L4 | Ada Lovelace | 24 GB GDDR6 | ~300 GB/s | FP8 | 72 W | No | Small models (up to ~8B at FP8), embeddings, rerankers, low power |
| NVIDIA L40S | Ada Lovelace | 48 GB GDDR6 | ~860 GB/s | FP8 | 350 W | No | 7–14B models at FP16; 30B-class at FP8; mixed AI and graphics |
| NVIDIA RTX PRO 6000 Blackwell Server Edition | Blackwell | 96 GB GDDR7 | ~1.6 TB/s | FP4 | 400–600 W (configurable) | No | Best value per GB today; 70B at FP8 across two cards; MIG up to 4 slices |
| NVIDIA H100 NVL | Hopper | 94 GB HBM3 | ~3.9 TB/s | FP8 | 350–400 W | Yes (bridge) | More users at once, fine-tuning, faster answers per user |
| NVIDIA H200 NVL | Hopper | 141 GB HBM3e | ~4.8 TB/s | FP8 | up to 600 W | Yes (bridge) | 70B at FP16 across two cards, long documents, many users |

Always check the NAI release notes and your server vendor's GPU qualification list, and speak with Nutanix before you order.

**A practical rule for a first test cluster:** buy fewer GPUs with more memory each. Two 96 GB cards give you more options than four 24 GB cards, because a model that does not fit on one GPU is the main reason pilots stall.

*Views are my own.*

tags: [GPUs, AI, Nutanix, NAI, NKP, Tokenomics]

## Sources

- [Nutanix — Run your AI in the cloud, or don't](https://www.nutanix.com/tech-center/blog/run-your-ai-in-the-cloud-or-dont-thats-the-point)
- [Nutanix — NAI 2.5 GPU support](https://www.nutanix.com/blog/nutanix-turns-ai-ambition-into-enterprise-control-and-customer-delight)
- [Nutanix — One platform for EUC and AI (vGPU)](https://www.nutanix.com/library/solution-briefs/nvidia-vdi-for-ai)
- [Nutanix — Orchestrating AI factory infrastructure](https://nutanix.com/blog/ochestrating-and-securing-ai-factory-infrastructure-with-nutanix-agentic-ai)
- [ServeTheHome — RTX PRO 6000 Blackwell family](https://www.servethehome.com/nvidia-rtx-pro-6000-blackwell-family-for-workstations-and-servers/)
- [AMAX — RTX PRO 6000 Server Edition vs L40S](https://www.amax.com/nvidia-rtx-pro-6000-blackwell-server-edition-vs-nvidia-l40s/)
- [Nutanix — Cloud Platform software options and licensing](https://www.nutanix.com/products/software-options)
- [Nutanix — NAI GPT deployment service description](https://www.nutanix.com/content/dam/nutanix/en/resources/service-descriptions/sd-nai-gpt-deployment.pdf)
- [D2iQ — DKP 2.7 resource requirements](https://docs.d2iq.com/dkp/2.7/resource-requirements)
- [D2iQ — DKP 2.6 resource requirements](https://docs.d2iq.com/dkp/2.6/resource-requirements)
