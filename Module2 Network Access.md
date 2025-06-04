# 📘 CCNA Module 2: Network Access

## 🧠 Module Objective:

Understand how switches operate, learn VLAN segmentation, implement trunking, and configure port security to control access.

---

## 🔀 2.1 Switching Concepts

- **MAC Address Table**: Switch learns MAC addresses on ports and forwards frames accordingly
    
- **Forwarding Methods**:
    
    - Store-and-Forward ✅ (Default)
        
    - Cut-Through
        
- **Collision Domains**: Switch breaks collision domains per port
    
- **Broadcast Domain**: VLANs define broadcast boundaries
    

---

## 🗂️ 2.2 VLANs (Virtual LANs)

- **Purpose**: Logically segment LANs for security and efficiency
    
- **Default VLAN**: VLAN 1
    
- **VLAN Range**: 1–4094
    

### VLAN Configuration (Lab)

```plaintext
Switch(config)#vlan 10
Switch(config-vlan)#name HR
Switch(config)#interface fa0/1
Switch(config-if)#switchport access vlan 10
```

### Show VLANs

```plaintext
Switch#show vlan brief
```

---

## 🔄 2.3 VLAN Trunking

- **802.1Q**: Trunking protocol adds tags to frames
    
- **Trunk Ports**: Allow multiple VLANs
    

### Trunk Config

```plaintext
Switch(config)#interface g0/1
Switch(config-if)#switchport mode trunk
```

### Show Trunk Status

```plaintext
Switch#show interfaces trunk
```

---

## 🧷 2.4 Inter-VLAN Routing

### Router-on-a-Stick Configuration

```plaintext
Router(config)#interface g0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
```

### Enable Trunk on Switch Port to Router

```plaintext
Switch(config)#interface g0/1
Switch(config-if)#switchport mode trunk
```

---

## 🌳 2.5 STP (Spanning Tree Protocol)

- **Purpose**: Prevent Layer 2 loops by blocking redundant links
    
- **STP States**:
    
    - Blocking
        
    - Listening
        
    - Learning
        
    - Forwarding
        
    - Disabled
        

### STP Command

```plaintext
Switch#show spanning-tree
```

---

## 🔐 2.6 Port Security

- Prevent unauthorized devices from connecting to switch ports
    
- **Sticky MAC**: Automatically learns MAC and stores in config
    
- **Violation Modes**:
    
    - Protect (drops traffic)
        
    - Restrict (logs violations)
        
    - Shutdown (default; disables port)
        

### Port Security Config

```plaintext
Switch(config)#interface fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport port-security
Switch(config-if)#switchport port-security maximum 1
Switch(config-if)#switchport port-security violation shutdown
Switch(config-if)#switchport port-security mac-address sticky
```

### Show Port Security

```plaintext
Switch#show port-security
```

---

## 🧪 2.7 Quick Labs

### Create VLANs and Assign Ports

```plaintext
Switch(config)#vlan 20
Switch(config-vlan)#name SALES
Switch(config)#interface range fa0/2-3
Switch(config-if-range)#switchport access vlan 20
```

### Enable Trunk and Connect Router

```plaintext
Switch(config)#interface g0/1
Switch(config-if)#switchport mode trunk
Router(config)#interface g0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 192.168.20.1 255.255.255.0
```

---

## 📝 Review Questions

1. What does STP prevent?
    
    - A) MAC address flooding
        
    - B) IP conflicts
        
    - C) Layer 2 loops ✅
        
    - D) VLAN tagging
        
2. What command sets a switchport to trunk?
    
    - A) switchport vlan trunk
        
    - B) switchport mode trunk ✅
        
    - C) switchport encapsulation 802.1Q
        
    - D) switchport trunk enable
        
3. What happens when port security violation occurs (default)?
    
    - A) Log only
        
    - B) Disable port ✅
        
    - C) Drop frame silently
        
    - D) Reboot switch
        

---

## 🧠 Summary

- VLANs segment Layer 2 networks logically
    
- Trunking allows VLANs to traverse links
    
- STP ensures loop-free topologies
    
- Port security helps mitigate unauthorized access
    

---

## 🔁 Next Module: IP Connectivity → Routing Fundamentals