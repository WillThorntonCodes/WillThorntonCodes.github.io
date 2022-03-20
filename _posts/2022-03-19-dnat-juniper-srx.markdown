layout: post
title: "How to Create a Destination NAT on a Juniper SRX"
date: YYYY-MM-DD hh:mm:ss -0000
categories: JUNIPER 

Use a destination NAT or _DNAT_ to send traffic destined to one endpoint to different endpoint IP without user intervention. This is useful in cases where the destination endpoint IP was changed and you do not want to change the statically set old IP on many devices. A DNAT can be a stopgap until the static IP is manually set on all devices then the DNAT can be removed.

```sh
set security nat destination pool TEST-DNAT address 3.3.3.1/32
set security nat destination rule-set rs1 from zone trust
set security nat destination rule-set rs1 rule r1 match destination-address 2.2.2.1/32
set security nat destination rule-set rs1 rule r1 then destination-nat pool TEST-DNAT
```
