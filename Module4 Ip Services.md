# 📘 CCNA Module 4: IP Services

## 🧠 Module Objective:

Master key IP services used in enterprise networks including DHCP, NAT, Syslog, NTP, and DNS basics. These are critical for real-world operations and exam success.

---

## 🧾 4.1 DHCP (Dynamic Host Configuration Protocol)

- Automatically assigns IP addresses to hosts
    
- Saves manual configuration time
    
- DHCP messages: Discover, Offer, Request, Acknowledge (DORA)
    

### DHCP Server on Cisco Router:

```plaintext
Router(config)#ip dhcp pool LANPOOL
Router(dhcp-config)#network 192.168.1.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.1.1
Router(dhcp-config)#dns-server 8.8.8.8
Router(dhcp-config)#lease 7
Router(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.10
```

---

## 🔁 4.2 NAT (Network Address Translation)

- **Purpose**: Translate private IPs to public IPs
    
- Types:
    
    - Static NAT (1-to-1)
        
    - Dynamic NAT (from a pool)
        
    - PAT (Overload)
        

### PAT Example:

```plaintext
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)#ip nat inside source list 1 interface g0/1 overload
Router(config)#interface g0/0
Router(config-if)#ip nat inside
Router(config)#interface g0/1
Router(config-if)#ip nat outside
```

### Verification:

```plaintext
Router#show ip nat translations
Router#show ip nat statistics
```

---

## ⏱️ 4.3 NTP (Network Time Protocol)

- Keeps all devices synchronized with a time source
    
- Critical for logging, certificates, etc.
    

### Config:

```plaintext
Router(config)#ntp server 192.168.1.100
Router(config)#clock timezone IST 5 30
```

### Show Time:

```plaintext
Router#show clock
```

---

## 📡 4.4 DNS Basics

- Domain Name System: Resolves names to IPs
    
- Typically provided by ISPs or internal servers
    

### Static DNS Server:

```plaintext
Router(config)#ip name-server 8.8.8.8
Router(config)#ip domain-lookup
```

---

## 🪵 4.5 Syslog & Logging

- Centralized log collection and monitoring
    
- Levels: 0 (emergencies) to 7 (debugging)
    

|Level|Description|
|---|---|
|0|Emergency|
|1|Alert|
|2|Critical|
|3|Error|
|4|Warning|
|5|Notification|
|6|Informational|
|7|Debug|

### Syslog Setup:

```plaintext
Router(config)#logging 192.168.1.50
Router(config)#logging trap informational
```

---

## 🧪 4.6 Quick Labs

### Configure DHCP:

```plaintext
Router(config)#ip dhcp excluded-address 192.168.10.1 192.168.10.10
Router(config)#ip dhcp pool OFFICE
Router(dhcp-config)#network 192.168.10.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.10.1
```

### NAT Overload Lab:

```plaintext
Router(config)#access-list 1 permit 10.0.0.0 0.255.255.255
Router(config)#ip nat inside source list 1 interface g0/1 overload
```

---

## 📝 Review Questions

1. Which NAT type allows many-to-one translation?
    
    - A) Static NAT
        
    - B) Dynamic NAT
        
    - C) PAT ✅
        
    - D) Overload NAT
        
2. What command enables logging to a remote server?
    
    - A) logging buffered
        
    - B) logging console
        
    - C) logging host
        
    - D) logging ✅
        
3. DHCP uses which message flow?
    
    - A) DRAO
        
    - B) OARD
        
    - C) DORA ✅
        
    - D) DOAR
        

---

## 🧠 Summary

- DHCP automates host IP assignment
    
- NAT translates between private and public IPs
    
- NTP and Syslog are vital for enterprise management
    
- DNS enables human-friendly network access
    

---

## 🔁 Next Module: Security Fundamentals → AAA, ACLs, Device Hardening