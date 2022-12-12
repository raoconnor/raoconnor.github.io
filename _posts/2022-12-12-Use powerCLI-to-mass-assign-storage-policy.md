
# Use PowerCLI to mass assign tags and storage based policy management

Storage Based Policy Management (SBPM) has been around for a while, when working with storage tiers, metro clusters, or vSAN, it’s either fundamental or best practice.

But clicking on anything more than half a dozen vms and datastores is a bit too much hard work.

Using a mix of PowerCLI and the web client we can save a ton of clicking without making things to complicated.
This is not a script just working with simple foreach loops and cmdlets

We will create and assign tags to datastores bases on their location (or tier), create a policy and use a rule to automatically assign the policy to any datastore with a specific tag.

After this we will assign to virtual machines tags ( this is optional) and  the storage policy. This will allow us see from the Web client if vms are on the correct datastore.


![This is an image](https://raoconnor.github.io/docs/assets/images/sbpm-1.png)


## 1. Create the categories and tags required

From the Tags and Custom Attributes section in the vSphere Client, create first the category. 

![This is an image](https://raoconnor.github.io/docs/assets/images/sbpm-2a.png)

Then create tags that belong to that category

![This is an image](https://raoconnor.github.io/docs/assets/images/sbpm-2.png)



## 2. Create the storage policy and rules

With storage polices a tag based rule will automatically apply a policy based on a tag, just think of this a descriptive meta data.

From VM Storage Policies create a new VM storage policy
Create a Tag based placement rule for each tag type

![This is an image](https://raoconnor.github.io/docs/assets/images/sbpm-3.png)

Assign the tags to each datastore, the rule will be automatically applied to the datastore


## Add Tags and Storage Policies to Datastores
List Tags of a certain category

> Get-Tag | Where { $_.Category -like "StorageAffinity"} | Select Name, Category

Add tags to groups of datastores

Now this depends how you have named the datastores, but just use the above snipits and change  Get-VM to Get-Datastore

for example

> $datastores = Get-datastore | Where {$_.Name -like "*SSD*"}

or, as I am a fan of organising the different LUN types into folders

> $datastores = Get-Folder <folder name> | Get-datastore

then add the tag, the rule created earlier will then apply the policy

> $Tag = Get-Tag STORAGE-AFFINITY-SITE-1
> foreach ($datstore in $datastores { New-TagAssignment -Tag $Tag -Entity $datastore}

## Add Tags and Storage Policies to virtual machines
Import the list of vms to apply the SBPM using one of the following methods

There are a number of methods of getting your vms into a the $vms variable

List vms with Name
> $vms = Get-VM | Where {$_.Name -like "*web*"}

or

Select vms in Folder
>$vms = Get-Folder <folder name> | Get-vm

or

For very large groups use the fast get-view method

> $f = Get-Folder <"folder name">
> $folder = (Get-Folder $f | Get-View)
> $foldervms = Get-View -SearchRoot $folder.MoRef -ViewType "VirtualMachine" | Select Name

or

Select vms from text file (change the path to match yours)
> $vms = Get-Content C:\PowerCLI\Input\vmlist.txt | foreach {Get-VM $_}

or

Select vms in a datastore
> foreach ($vm in $vms){ Get-vm $vm | Select Name, @{N="Datastore";E={Get-Datastore -VM $_}}}

# Add Tags to multiple vms
Keep in mind that the vms tags just help in identification and the storage policy is applied directly
The tags are a double check, but not essential for virtual machine policy management

> $Tag = Get-Tag VM-AFFINITY-SITE-1
> foreach ($vm in $vms) { New-TagAssignment -Tag $Tag -Entity $vm}

Check they are applied
> foreach ($vm in $vms) { Get-TagAssignment -Entity $vm | Select Entity, Tag}

 
Add storage policy to multiple vms

for a single vm it would be this

> Get-vm <vm> | Set-SpbmEntityConfiguration -StoragePolicy <storage policy>
> Get-vm <vm> | Get-SpbmEntityConfiguration

for multiple vms ( that you have loaded into $vm – see above )

> foreach ($vm in $vms) {Get-vm $vm | Set-SpbmEntityConfiguration -StoragePolicy SBPM-PROD-SITE-1}
> foreach ($vm in $vms) {Get-vm $vm | Get-SpbmEntityConfiguration}

If you want to look at all vms in the Datacenter then reload the $vms variable with all the vms (ie: $vms  = Get-vm) then run the Get-SpbmEntityConfiguration line again.

> foreach ($vm in $vms) {Get-vm $vm | Set-SpbmEntityConfiguration -StoragePolicy <SBPM-POLICY>}

 
Be sure to add the VM Storage Policies Compliance column in the web client, so you can see if vms are compliant

I’ve created a couple of powercli scripts that can be used to filter vms by tag and then see the policy and compliance level, or to simple check policy and compliance on all vms.
  
[Get-VmTagAndSbpmInfo](https://github.com/raoconnor/Get-VmTagAndSbpmInfo)
[Get-VmSbpm](https://github.com/raoconnor/Get-VmSbpm)
