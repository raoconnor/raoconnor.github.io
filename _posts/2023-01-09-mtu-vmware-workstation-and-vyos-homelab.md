---
title: "mtu, vmware workstation and vyos router in a homelab"
date: 2023-01-09
---

For those wanting to play with NSX-T in your homelab, you will be aware that a MTU of 1600 as a minumum is needed.
If you are runnng the lab in VMware workstation, and using VYOS as the router, you may run into the same issue as me.

Even after setting the virtual network cards in your host OS to use jumbo frames, (This is the OS, Windows 10/11 or Linux where you have installed workstation, not a vm but your desktop OS)

[Enable Jumbo Frames on Windows Host](https://docs.vmware.com/en/VMware-Workstation-Player-for-Windows/16.0/com.vmware.player.win.using.doc/GUID-EAF6324F-CD1E-4609-8525-0135F09F0D13.html)

Jumbo frames doesn't work 

´´´
vmkping -d -s 8972 x.x.x.x
vmkping -I vmkX x.x.x.x -d -s 8972
´´´

[Testing VMkernel network connectivity with the vmkping command](https://kb.vmware.com/s/article/1003728)


It seems the issue is the VYOS virtual router, and so far no one has a solution (this applies to running VYOS inside workstation not ESXi)
I switched the router to pfsense any everything worked ok

Just be aware that pfsense has built in firewall rules and you need to understand those and remove/create rules for complex routing etc.
