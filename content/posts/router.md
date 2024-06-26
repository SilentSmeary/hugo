---
title: "Cisco Router"
date: 2024-06-24
url: /router 
image: https://www.bleepstatic.com/content/hl-images/2020/10/20/Cisco.jpg
categories:
  - Cisco
  - Networking
series:
 - 2024 
draft: false
---
## Pre-Information
The code below in the configuration for a Cisco 1921 with a class C Network where the router has been given the IP address 172.16.0.1. This has got a DHCP range of 100 IP address range address. There are 0 VLAN's currently configured but the plan is to have a VLAN for the servers so that only allowed devices are able to speak with the server. We are also using a Cisco switch that is running POE for our AP device.
## Current Config

```
Current configuration : 1583 bytes
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname Router
!
boot-start-marker
boot-end-marker
!
!
enable secret 4 56jyXs.RSLFQFX5Ebzwqm0eXTwHAtDmINcDLgnOqA16
!
no aaa new-model
memory-size iomem 5
!
ip cef
!
!
!
ip dhcp excluded-address 172.16.0.1 172.16.0.99
ip dhcp excluded-address 172.16.0.200 172.16.0.254
!
ip dhcp pool pool1
 network 172.16.0.0 255.255.0.0
 default-router 172.16.0.1
 dns-server 172.16.0.4 8.8.8.8
!
!
!
no ipv6 cef
multilink bundle-name authenticated
!
!
!
license udi pid CISCO1921/K9 sn FCZ1809C3MT
!
!
!
!
!
!
!
!
interface Embedded-Service-Engine0/0
 no ip address
 shutdown
!
interface GigabitEthernet0/0
 ip address dhcp
 ip nat outside
 ip virtual-reassembly in
 duplex auto
 speed auto
!
interface GigabitEthernet0/1
 ip address 172.16.0.1 255.255.0.0
 ip nat inside
 ip virtual-reassembly in
 duplex auto
 speed auto
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip nat pool Internet 192.168.9.20 192.168.9.20 netmask 255.255.255.0
ip nat inside source list 100 pool Internet overload
ip route 0.0.0.0 0.0.0.0 192.168.9.1
!
access-list 100 deny   ip 172.16.0.0 0.0.255.255 172.16.0.0 0.0.255.255
access-list 100 permit ip 172.16.0.0 0.0.255.255 any
!
!
!
control-plane
!
!
!
line con 0
line aux 0
line 2
 no activation-character
 no exec
 transport preferred none
 transport output pad telnet rlogin lapb-ta mop udptn v120 ssh
 stopbits 1
line vty 0 4
 password telnet
 login
 transport input telnet
!
scheduler allocate 20000 1000
!
end
```

## Common Commands
### Save current config to startup
```
Router> enable
Router# copy running-config startup-config
Destination filename [startup-config]? <Enter>
```
### Show the running config
```
Router> enable
Router# show running-config
```
### Show IP interface brief
```
Router# show ip interface brief
```
### Router Restart
```
Router> enable
Router# reload
```
### Set an IP address
```
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 10.0.0.1 255.0.0.0
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# exit
```
### Changing the hostname
```
Router> enable
Router# configure terminal
Router(config)# hostname MyRouter
```
### Setting a console password
```
Router> enable
Router# configure terminal
Router(config)# line console 0
Router(config-line)# password myconsolepassword
Router(config-line)# login
Router(config-line)# exit
```