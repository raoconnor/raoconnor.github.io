
## Use PowerCLI to mass assign tags and storage based policy management

Storage Based Policy Management (SBPM) has been around for a while, when working with storage tiers, metro clusters, or vSAN, it’s either fundamental or best practice.

But clicking on anything more than half a dozen vms and datastores is a bit too much hard work.

Using a mix of PowerCLI and the web client we can save a ton of clicking without making things to complicated.
This is not a script just working with simple foreach loops and cmdlets

We will create and assign tags to datastores bases on their location (or tier), create a policy and use a rule to automatically assign the policy to any datastore with a specific tag.

After this we will assign to virtual machines tags ( this is optional) and  the storage policy. This will allow us see from the Web client if vms are on the correct datastore.


![This is an image](https://raoconnor.github.io/docs/assets/images/sbpm-1.png)
