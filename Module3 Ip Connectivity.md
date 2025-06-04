# 📘 CCNA Module 3: IP Connectivity

## 🧠 Module Objective:

Understand routing principles, configure static and dynamic routing protocols (RIP, OSPF, EIGRP), and troubleshoot IP connectivity.

---

## 🔀 3.1 Routing Concepts

- **Routing**: Process of selecting paths for traffic in a network
    
- **Router Functions**:
    
    - Maintains routing table
        
    - Forwards packets based on best path
        
- **Administrative Distance (AD)**:
    
    - Trustworthiness of route source
        

|Route Type|AD Value|
|---|---|
|Connected|0|
|Static|1|
|EIGRP (Internal)|90|
|OSPF|110|
|RIP|120|

---

## 📍 3.2 Static Routing

### Syntax:

```plaintext
Router(config)#ip route <DESTINATION> <MASK> <NEXT_HOP>
```

### Example:

```plaintext
Router(config)#ip route 192.168.2.0 255.255.255.0 192.168.1.2
```

---

## 🔁 3.3 RIP (Routing Information Protocol)

- Distance-vector protocol
    
- Max hop count = 15
    
- Broadcasts entire routing table every 30 seconds
    

### Configuration:

```plaintext
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 192.168.1.0
Router(config-router)#network 192.168.2.0
```

### Verification:

```plaintext
Router#show ip route
Router#show ip protocols
```

---

## 📡 3.4 OSPF (Open Shortest Path First)

- Link-state protocol
    
- Uses SPF algorithm (Dijkstra)
    
- Organizes routers into areas (usually Area 0)
    

### Basic Config (Single Area):

```plaintext
Router(config)#router ospf 1
Router(config-router)#network 192.168.1.0 0.0.0.255 area 0
Router(config-router)#network 192.168.2.0 0.0.0.255 area 0
```

### Verification:

```plaintext
Router#show ip ospf neighbor
Router#show ip ospf interface
```

---

## ⚡ 3.5 EIGRP (Enhanced Interior Gateway Routing Protocol)

- Advanced distance-vector protocol (Cisco proprietary)
    
- Uses DUAL algorithm
    

### Configuration:

```plaintext
Router(config)#router eigrp 100
Router(config-router)#network 192.168.1.0 0.0.0.255
Router(config-router)#network 192.168.2.0 0.0.0.255
```

### Verification:

```plaintext
Router#show ip eigrp neighbors
Router#show ip eigrp topology
```

---

## 🧪 3.6 Quick Labs

### Static Routing:

```plaintext
Router1(config)#ip route 10.0.0.0 255.0.0.0 192.168.1.2
```

### RIP Lab:

```plaintext
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 10.0.0.0
```

### OSPF + EIGRP Lab

```plaintext
Router(config)#router ospf 1
Router(config-router)#network 10.0.0.0 0.255.255.255 area 0

Router(config)#router eigrp 10
Router(config-router)#network 10.0.0.0 0.255.255.255
```

---

## 🧰 3.7 Troubleshooting Commands

```plaintext
Router#ping <IP>
Router#traceroute <IP>
Router#show ip interface brief
Router#show ip route
Router#debug ip rip
Router#debug ip ospf events
```

---

## 📝 Review Questions

1. What is the AD of a static route?
    
    - A) 0
        
    - B) 1 ✅
        
    - C) 90
        
    - D) 120
        
2. Which protocol uses DUAL algorithm?
    
    - A) RIP
        
    - B) OSPF
        
    - C) EIGRP ✅
        
    - D) BGP
        
3. How many hops can RIP support?
    
    - A) 16
        
    - B) 15 ✅
        
    - C) 255
        
    - D) 10
        

---

## 🧠 Summary

- Static routes are manually defined
    
- RIP, OSPF, EIGRP are dynamic routing protocols with unique metrics
    
- AD determines route preference
    
- Use `ping`, `traceroute`, `show` and `debug` for diagnostics
    

---

## 🔁 Next Module: IP Services → DHCP, NAT, NTP, Syslog