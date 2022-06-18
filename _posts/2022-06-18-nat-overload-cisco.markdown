---
layout: post
title: "How to Create NAT overload on Cisco"
date: 2022-06-18 14:34:00 -0000
categories: CISCO VIRTUAL-LAB
---
Use NAT overload or _Port Address Translation(PAT)_ to allow many-to-one address translation for your network clients. Meaning many internal clients can use the one IP address to access another network like the internet. 

In my lab, I will be using a DHCP address assigned by my GNS3 default-gateway. In most situations a static IP assigned by an ISP will be used.

On WAN interface set up IP address.
```sh
R1(config)#interface FastEthernet 0/0
R1(config-if)#ip address dhcp
```

Set NAT on WAN interface
```sh
R1(config-if)#ip nat outside
```

Set LAN IP on internal interface
```sh
R1(config)#interface FastEthernet 0/1
R1(config-if)#ip address 192.168.1.1 255.255.255.0
```

Set NAT inside on LAN interface
```sh
R1(config-if)#ip nat inside
```

Create access-list to allow internal LAN traffic
```sh
R1(config)#access-list 10 permit 192.168.1.0 0.0.0.255 
```

Set default route which will point traffic to the next hop of the GNS3 gateway
```sh
R1(config)#ip route 0.0.0.0 0.0.0.0 192.168.122.1
```

NAT statement that puts it all together
```sh
ip nat inside source list 10 interface FastEthernet0/0 overload
```

This last statement says to NAT any outgoing traffic that matches the access-list called "10" and use outside interface FastEthernet0/0 to egress. The overload flag will use Port Address Translation to enable the many-to-one connection.

Save your work and happy labbing!
