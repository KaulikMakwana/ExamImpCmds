# 📘 CCNA Module 1: Networking Fundamentals

## 🧠 Module Objective:

Understand the foundational principles of networking, including the OSI and TCP/IP models, IP addressing, subnetting, and key network devices.

---

## 📚 1.1 OSI vs TCP/IP Model

### OSI Model (7 Layers)

|Layer|Name|Function|
|---|---|---|
|7|Application|Interface to user apps|
|6|Presentation|Data format/encoding|
|5|Session|Session control|
|4|Transport|Reliable delivery (TCP)|
|3|Network|Routing/IP addressing|
|2|Data Link|MAC addressing, frames|
|1|Physical|Bits, cables, signals|

### TCP/IP Model (4 Layers)

|Layer|OSI Equivalent|
|---|---|
|4|Application (7,6,5)|
|3|Transport (4)|
|2|Internet (3)|
|1|Network Access (2,1)|

---

## ⚙️ 1.2 Data Flow Types

- **Unicast** – One sender to one receiver
    
- **Broadcast** – One to all (within subnet)
    
- **Multicast** – One to many (specific group)
    

---

## 🖧 1.3 Network Types

- **LAN** – Local network (e.g., office)
    
- **WAN** – Wide-area network (e.g., Internet)
    
- **WLAN** – Wireless LAN
    
- **MAN** – Metropolitan network
    
- **PAN** – Personal network (e.g., Bluetooth)
    

---

## 🔌 1.4 Cabling & Devices

### Cable Types

- **Straight-through** – PC to Switch
    
- **Crossover** – Switch to Switch or PC to PC
    
- **Rollover** – Console cable
    

### Devices

- **Hub** – Broadcast device (Layer 1)
    
- **Switch** – MAC aware (Layer 2)
    
- **Router** – IP routing (Layer 3)
    
- **Access Point** – Wireless bridge
    

---

## 🌐 1.5 IPv4 Addressing

### Classes

|Class|Range|Default Subnet Mask|
|---|---|---|
|A|1.0.0.0 - 126|255.0.0.0|
|B|128 - 191|255.255.0.0|
|C|192 - 223|255.255.255.0|

### Reserved Addresses

- **127.0.0.1** – Loopback
    
- **169.254.x.x** – APIPA
    
- **255.255.255.255** – Broadcast
    

### Private IPs

- **Class A:** 10.0.0.0/8
    
- **Class B:** 172.16.0.0 – 172.31.255.255
    
- **Class C:** 192.168.0.0/16
    

---

## 📏 1.6 Subnetting Basics

### Formula

- Hosts per subnet = 2h - 2 (h = host bits)
    
- Subnets = 2s (s = subnet bits)
    

### Example:

- /24 = 255.255.255.0 = 256 IPs, 254 usable
    
- /30 = 255.255.255.252 = 4 IPs, 2 usable (Point-to-Point)
    

---

## 🧪 1.7 Quick Labs (CLI)

```plaintext
PC> ipconfig      # Show IP config (Windows)
Router#show ip interface brief
Router#ping 8.8.8.8
```

### Subnetting Exercise

- Subnet 192.168.1.0/24 into 4 subnets:
    
    - New mask: /26
        
    - Each subnet: 64 IPs (62 usable)
        

---

## 📝 Review Questions

1. What layer of OSI is responsible for routing?
    
    - A) Data Link
        
    - B) Transport
        
    - C) Network ✅
        
    - D) Session
        
2. What is the subnet mask of a /30?
    
    - A) 255.255.255.0
        
    - B) 255.255.255.192
        
    - C) 255.255.255.252 ✅
        
    - D) 255.255.255.248
        
3. Which of the following is a private IP?
    
    - A) 8.8.8.8
        
    - B) 172.31.200.5 ✅
        
    - C) 223.1.1.1
        
    - D) 192.0.2.1
        

---

## 🧠 Summary

- OSI model has 7 layers, TCP/IP has 4
    
- IP addressing + subnetting is core for routing
    
- Devices operate at different layers and have unique roles
    
- Subnetting and binary calculations are exam-heavy topics
    

---

## 🔁 Next Module: Network Access → VLANs, STP, Inter-VLAN