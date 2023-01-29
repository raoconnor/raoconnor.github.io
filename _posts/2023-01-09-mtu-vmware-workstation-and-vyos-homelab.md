---
title: "mtu, vmware workstation and vyos router in a homelab"
date: 2023-01-09
---

For those wanting to play with NSX-T in your homelab, you will be aware that a MTU of 1600 as a minumum is needed.
If you are runnng the lab in VMware workstation, and using VYOS as the router, you may run into the same issue as me.

Even after setting the virtual network cards in your host OS to use jumbo frames, (This is the OS, Windows 10/11 or Linux where you have installed workstation, not a vm but your desktop OS)

[Enable Jumbo Frames on Windows Host](https://docs.vmware.com/en/VMware-Workstation-Player-for-Windows/16.0/com.vmware.player.win.using.doc/GUID-EAF6324F-CD1E-4609-8525-0135F09F0D13.html)

Jumbo frames doesn't work 

```
vmkping -d -s 8972 x.x.x.x
vmkping -I vmkX x.x.x.x -d -s 8972
```

[Testing VMkernel network connectivity with the vmkping command](https://kb.vmware.com/s/article/1003728)


It seems the issue is the VYOS virtual router, and so far no one has a solution (this applies to running VYOS inside workstation not ESXi)

First I switched the virtual router to pfsense, which allowed me to do jumbo frames, vlan etc.
However pfsense is a complex animal it has a built in firewall and setting up routing can be complex. 
Finally I purchased a small phycial router, (TP-Link ER605) and used that along with a small switch a had at home, the router itself doensn't support jumbo frames but I use the switch to connect hosts that need to use jumbo frames and keep those on the same network.

