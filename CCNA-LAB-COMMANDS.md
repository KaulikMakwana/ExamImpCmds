# CCNA Exam Important Commands – Compiled Notes
## Table of Contents

- [Static Routing](#static-routing)
- [Three Router Static Routes Example](#three-router-static-routes-example)
- [Backup & Restore NVRAM (TFTP)](#backup--restore-nvram-tftp)
- [ACLs (Access Control Lists)](#acls-access-control-lists)
- [EIGRP Configuration](#eigrp-configuration)
- [OSPF Configuration](#ospf-configuration)
- [RIP Configuration](#rip-configuration)
- [NAT (Network Address Translation)](#nat-network-address-translation)
- [IPv6 Configuration & RIPng](#ipv6-configuration--ripng)
- [VLAN Configuration](#vlan-configuration)
- [Inter-VLAN Routing](#inter-vlan-routing)
- [Port Blocking & Port Security](#port-blocking--port-security)
- [SSH Configuration](#ssh-configuration)
- [IOS Backup & Restore](#ios-backup--restore)
- [Extra Router/Switch Commands](#extra-routerswitch-commands)
- [not_dec.txt (miscellaneous)](#not_dectxt-miscellaneous)

---

## Static Routing

```plaintext
router 1

Continue with configuration dialog? [yes/no]: no
Router>enable 
Router#configure terminal 
Router(config)#hostname r1
r1(config)#interface gigabitEthernet 0/0
r1(config-if)#ip address 192.168.10.1 255.255.255.0
r1(config-if)#no shutdown 
r1(config-if)#exit
r1(config)#interface gigabitEthernet 0/1
r1(config-if)#ip address 192.168.20.1 255.255.255.0
r1(config-if)#no shutdown 
r1(config-if)#^Z
r1#wr
r1#show ip route
r1#configure terminal 
r1(config)#ip route 192.168.30.0 255.255.255.0 192.168.20.2
r1(config)#^Z
r1#wr
r1#show ip route

router 2

Continue with configuration dialog? [yes/no]: no
Router>enable 
Router#configure terminal 
Router(config)#hostname r2
r2(config)#interface gigabitEthernet 0/0
r2(config-if)#ip address 192.168.30.1 255.255.255.0
r2(config-if)#no shutdown 
r2(config-if)#exit
r2(config)#interface gigabitEthernet 0/1
r2(config-if)#ip address 192.168.20.2 255.255.255.0
r2(config-if)#no shutdown 
r2(config-if)#^Z
r2#wr
r2#show ip route
r2#configure terminal 
r2(config)#ip route 192.168.10.0 255.255.255.0 192.168.20.1
r2(config)#^Z
r2#wr
r2#show ip route
```

---

## Three Router Static Routes Example

```plaintext
r1(config)#ip route 192.168.30.0 255.255.255.0 192.168.20.2
r1(config)#ip route 192.168.40.0 255.255.255.0 192.168.20.2
r1(config)#ip route 192.168.50.0 255.255.255.0 192.168.20.2
r1(config)#^Z

r2(config)#ip route 192.168.10.0 255.255.255.0 192.168.20.1
r2(config)#ip route 192.168.50.0 255.255.255.0 192.168.40.2

r3(config)#ip route 192.168.10.0 255.255.255.0 192.168.40.1
r3(config)#ip route 192.168.20.0 255.255.255.0 192.168.40.1
r3(config)#ip route 192.168.30.0 255.255.255.0 192.168.40.1
```

---

## Backup & Restore NVRAM (TFTP)

```plaintext
backup of nvram

kaal#ping 10.0.0.100 (tftp server ip)
kaal#copy startup-config tftp: 
Address or name of remote host []? 10.0.0.100
Destination filename [kaal-confg]? kaal

restore of backup on new router

[yes/no]: no

Router>enable
Router#configure terminal
Router(config)#interface fastEthernet 0/0
Router(config-if)#ip address 10.0.0.1 255.0.0.0
Router(config-if)#no shutdown 
Router(config-if)#exit
Router(config)#exit
Router#ping 10.0.0.100
Router#copy tftp: startup-config 
Address or name of remote host []? 10.0.0.100
Source filename []? kaal
Destination filename [startup-config]? press enter
Router#reload
  now your router start with old configure
```

---

## ACLs (Access Control Lists)

### Standard ACL on Router 1

```plaintext
r1(config)#access-list 11 permit host 10.0.0.2
r1(config)#access-list 11 deny host 10.0.0.3
r1(config)#access-list 11 deny host 10.0.0.4
r1(config)#access-list 11 permit host 10.0.0.5
r1(config)#interface gigabitEthernet 0/0
r1(config-if)#ip access-group 11 in
r1#show access-lists 
```

### Extended ACL on Router 2

```plaintext
r2(config)#access-list 111 permit icmp any any
r2(config)#access-list 111 permit tcp host 10.0.0.2 host 30.0.0.2 eq 80
r2(config)#access-list 111 permit tcp host 10.0.0.3 host 30.0.0.2 eq 21
r2(config)#access-list 111 permit tcp host 10.0.0.5 host 30.0.0.2 eq 21
r2(config)#access-list 111 permit tcp host 10.0.0.5 host 30.0.0.2 eq 80
r2(config)#int gi 0/0
r2(config-if)#ip access-group 111 out
r2(config-if)#exit
```

---

## EIGRP Configuration

```plaintext
r1(config)#router eigrp 10
r1(config-router)#network 192.168.10.0 0.0.0.255
r1(config-router)#network 192.168.20.0 0.0.0.255

r2(config)#router eigrp 10
r2(config-router)#network 192.168.20.0 0.0.0.255
r2(config-router)#network 192.168.30.0 0.0.0.255
r2(config-router)#network 192.168.40.0 0.0.0.255

r3(config)#router eigrp 10
r3(config-router)#network 192.168.40.0 0.0.0.255
r3(config-router)#network 192.168.50.0 0.0.0.255

# Check status any router
r3#show ip route
r3#show ip protocols
r3#show ip eigrp neighbors 
r3#show ip eigrp topology
r3#show ip eigrp trafic
```

---

## OSPF Configuration

```plaintext
r1(config)#router ospf 10
r1(config-router)#network 192.168.10.0 0.0.0.255 area 1
r1(config-router)#network 192.168.20.0 0.0.0.255 area 1

r2(config)#router ospf 10
r2(config-router)#network 192.168.20.0 0.0.0.255 area 1
r2(config-router)#network 192.168.30.0 0.0.0.255 area 1
r2(config-router)#network 192.168.40.0 0.0.0.255 area 1

r3(config)#router ospf 10
r3(config-router)#network 192.168.40.0 0.0.0.255 area 1
r3(config-router)#network 192.168.50.0 0.0.0.255 area 1

# Check status any router
r3#show ip route
r3#show ip protocols
r3#show ip ospf neighbors 
r3#show ip ospf database
```

---

## RIP Configuration

```plaintext
r1>enable
r1#show ip route  (check routing table)
r1(config)#router rip
r1(config-router)#network 192.168.10.0
r1(config-router)#network 192.168.20.0

r2>enable
r2#show ip route  (check routing table)
r2(config)#router rip
r2(config-router)#network 192.168.20.0
r2(config-router)#network 192.168.30.0
r2(config-router)#network 192.168.40.0

r3>enable
r3#show ip route  (check routing table)
r3(config)#router rip
r3(config-router)#network 192.168.40.0
r3(config-router)#network 192.168.50.0
```

---

## NAT (Network Address Translation)

### Static NAT

```plaintext
r1>enable
r1#configure terminal 
r1(config)#access-list 11 permit host 10.0.0.2
r1(config)#access-list 11 permit host 10.0.0.3
r1(config)#int gi 0/0
r1(config-if)#ip access-group 11 in
r1(config-if)#exit
r1(config)#ip nat inside source static 10.0.0.2 20.0.0.10 
r1(config)#ip nat inside source static 10.0.0.3 20.0.0.11
r1(config)#int gi 0/0
r1(config-if)#ip nat inside
r1(config-if)#int gi 0/1
r1(config-if)#ip nat outside
r1(config-if)#^Z

# Status check
r1#show ip nat translations 
r1#show ip nat statistics 
```

### Dynamic NAT

```plaintext
r1(config)#access-list 11 permit any 
r1(config)#interface gigabitEthernet 0/0
r1(config-if)#ip access-group 11 in
r1(config-if)#exit
r1(config)#ip nat pool mak 20.0.0.10  20.0.0.12 netmask 255.0.0.0
r1(config)#ip nat inside source list 11 pool mak
r1(config)#interface gi 0/0
r1(config-if)#ip nat inside
r1(config-if)#interface gi 0/1
r1(config-if)#ip nat outside
r1(config-if)#^Z

# Status check
r1#show ip nat translations 
r1#show ip nat statistics 
r1#clear ip nat translation *
```

### Dynamic Overload (PAT)

```plaintext
r1(config)#access-list 11 permit any 
r1(config)#interface gigabitEthernet 0/0
r1(config-if)#ip access-group 11 in
r1(config-if)#exit
r1(config)#ip nat pool mak 20.0.0.10  20.0.0.10 netmask 255.0.0.0
r1(config)#ip nat inside source list 11 pool mak overload
r1(config)#interface gi 0/0
r1(config-if)#ip nat inside
r1(config-if)#interface gi 0/1
r1(config-if)#ip nat outside
r1(config-if)#^Z

# Status check
r1#show ip nat translations 
r1#show ip nat statistics 
r1#clear ip nat translation *
```

---

## IPv6 Configuration & RIPng

```plaintext
router 1

Router>enable 
Router#configure terminal 
Router(config)#hostname r1
r1(config)#ipv6 unicast-routing 
r1(config)#int gi 0/0
r1(config-if)#ipv6 add 2001::1/64
r1(config-if)#no sh
r1(config-if)#exit
r1(config)#int gi 0/1
r1(config-if)#ipv6 add 2002::1/64
r1(config-if)#no sh
r1(config-if)#exit 
r1(config)#ipv6 router rip abc
r1(config-rtr)#exit
r1(config)#int gi 0/0
r1(config-if)#ipv6 rip abc enable
r1(config-if)#int gi 0/1
r1(config-if)#ipv6 rip abc enable
r1(config-if)#^Z
r1#wr

# Check for status
r1#show ipv6 route 

router 2

Router>enable 
Router#configure terminal 
Router(config)#hostname r2
r2(config)#ipv6 unicast-routing 
r2(config)#int gi 0/0
r2(config-if)#ipv6 add 2003::1/64
r2(config-if)#no sh
r2(config-if)#exit
r2(config)#int gi 0/1
r2(config-if)#ipv6 add 2002::2/64
r2(config-if)#no sh
r2(config-if)#exit 
r2(config)#ipv6 router rip abc
r2(config-rtr)#exit
r2(config)#int gi 0/0
r2(config-if)#ipv6 rip abc enable
r2(config-if)#int gi 0/1
r2(config-if)#ipv6 rip abc enable
r2(config-if)#^Z
r2#wr

# Check for status
r2#show ipv6 route 
```

---

## VLAN Configuration

```plaintext
switch 1

Switch(config)#vlan 2 
Switch(config-vlan)#name batman
Switch(config-vlan)#exit
Switch(config)#vlan 3
Switch(config-vlan)#name sales
Switch(config-vlan)#^Z
Switch#show vlan
Switch#configure terminal
Switch(config)#interface fa 0/1
Switch(config-if)#switchport access vlan 2
Switch(config-if)#interface fa 0/2
Switch(config-if)#switchport access vlan 2
Switch(config-if)#interface fa 0/3
Switch(config-if)#switchport access vlan 3
Switch(config-if)#interface fa 0/4
Switch(config-if)#switchport access vlan 3
Switch(config-if)#^Z
Switch#show vlan

switch 2

Switch(config)#vlan 2 
Switch(config-vlan)#name batman
Switch(config-vlan)#exit
Switch(config)#vlan 3
Switch(config-vlan)#name sales
Switch(config-vlan)#^Z
Switch#show vlan
Switch#configure terminal
Switch(config)#interface fa 0/1
Switch(config-if)#switchport access vlan 2
Switch(config-if)#interface fa 0/2
Switch(config-if)#switchport access vlan 3
Switch(config-if)#exit
Switch(config)#interface gigabitEthernet 1/1
Switch(config-if)#switchport mode trunk 
```

---

## Inter-VLAN Routing

```plaintext
switch 

Switch>enable
Switch#configure terminal 
Switch(config)#hostname sw1
sw1(config)#vlan 2
sw1(config-vlan)#name mark
sw1(config-vlan)#exit
sw1(config)#vlan 3
sw1(config-vlan)#name sales
sw1(config-vlan)#exit
sw1(config)#int fa 0/1
sw1(config-if)#switchport access vlan 2
sw1(config-if)#int fa 0/2
sw1(config-if)#switchport access vlan 2
sw1(config-if)#int fa 0/3
sw1(config-if)#switchport access vlan 3
sw1(config-if)#int fa 0/4
sw1(config-if)#switchport access vlan 3
sw1(config-if)#exit
sw1(config)#int gi 1/1
sw1(config-if)#switchport mode trunk 
sw1(config-if)#^Z
sw1#wr
sw1#sh vlan 

router

Router>enable 
Router#configure terminal 
Router(config)#hostname r1
r1(config)#interface gigabitEthernet 0/0
r1(config-if)#no ip add
r1(config-if)#no sh
r1(config-if)#exit
r1(config)#interface gigabitEthernet 0/0.1
r1(config-subif)#encapsulation dot1Q 2
r1(config-subif)#ip address 10.0.0.1 255.0.0.0
r1(config-subif)#exit
r1(config)#interface gigabitEthernet 0/0.2
r1(config-subif)#encapsulation dot1Q 3
r1(config-subif)#ip address 20.0.0.1 255.0.0.0
r1(config-subif)#^Z
r1#sh run
r1#wr
```

---

## Port Blocking & Port Security

```plaintext
Switch>enable
Switch#configure terminal 
Switch(config)#hostname sw1
sw1(config)#interface range fastEthernet 0/1-24
sw1(config-if-range)#switchport mode access
sw1(config-if-range)#switchport port-security 
sw1(config-if-range)#switchport port-security mac-address sticky 
sw1(config-if-range)#switchport port-security maximum 1
sw1(config-if-range)#switchport port-security violation shutdown 
sw1(config-if-range)#^Z

# Ping all pc then check 
sw1#show mac-address-table
sw1#show running-config 
sw1#wr
```

---

## SSH Configuration

```plaintext
kaal(config)#ip domain-name ccna
kaal(config)#crypto key generate rsa
kaal(config)#ip ssh version 2
kaal(config)#line vty 0 15
kaal(config-line)#password vvv
kaal(config-line)#login
kaal(config-line)#login local
kaal(config-line)#transport input ssh
kaal(config-line)#exit
kaal(config)#username kaal password 1234
```

---

## IOS Backup & Restore

```plaintext
ios backup

kaal#show flash:
kaal#ping 10.0.0.100
kaal#copy flash tftp
Source filename []? c2800nm-advipservicesk9-mz.124-15.T1.bin
Address or name of remote host []? 10.0.0.100 (tftp server ip)
Destination filename [c2800nm-advipservicesk9-mz.124-15.T1.bin]? abc.bin

new ios from tftp

kaal#del c2800nm-advipservicesk9-mz.124-15.T1.bin
enter
enter
kaal#show flash: 

kaal#copy tftp flash
Address or name of remote host []? 10.0.0.100
Source filename []? c2800nm-ipbasek9-mz.124-8.bin
Destination filename [c2800nm-ipbasek9-mz.124-8.bin]?press enter
kaal#show flash:
kaal#reload

if two ios in your flash 
kaal#show flash:
kaal#configure terminal
kaal(config)#boot system flash c2800nm-ipbase-mz.123-14.T7.bin
kaal(config)#exit
kaal#copy running-config startup-config 
kaal#reload

kaal>enable
kaal#show version (new ios load)

no ios in your flash
rommon 1>tftpdnld
rommon 2 > IP_ADDRESS=10.0.0.1
rommon 3 > IP_SUBNET_MASK=255.0.0.0
rommon 4 > DEFAULT_GATEWAY=10.0.0.1
rommon 5 > TFTP_SERVER=10.0.0.100
rommon 6 > TFTP_FILE=c2800nm-advipservicesk9-mz.124-15.T1.bin
rommon 7 > set
rommon 8 > tftpdnld
Do you wish to continue? y/n:  [n]:  y
rommon 9>reset
(now your ios start)
```

---

## Extra Router/Switch Commands

```plaintext
r1#show cdp neighbors detail  

r2(config)#ip host r1 10.0.0.1
r2(config)#exit
r2#r1  (you can access router1 via telnet)

ssh

r1(config)#ip domain-name ccna
r1(config)#crypto key generate rsa 
r1(config)#line vty 0 15
r1(config-line)#password vvv
r1(config-line)#transport input ssh 
r1(config-line)#^Z
r1#wr

client router 2

r2#ssh -l r1 -v 1 10.0.0.1
```

---

## not_dec.txt (miscellaneous)

This file lists various tools/packages with the note "nothing appropriate."  
(Likely an inventory of tools not directly relevant or not available in the current environment.)

```plaintext
0trace: nothing appropriate.
accountsservice: nothing appropriate.
adwaita-icon-theme: nothing appropriate.
afflib-tools: nothing appropriate.
afl++-clang: nothing appropriate.
afl++-doc: nothing appropriate.
...
(And many more entries)
...
```

---
