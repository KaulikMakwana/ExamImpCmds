# 📘 CCNA Module 5: Security Fundamentals

## 🧠 Module Objective:

Understand how to secure network devices using AAA, access control lists (ACLs), secure remote access, and switch-level security techniques.

---

## 🔒 5.1 AAA (Authentication, Authorization, Accounting)

- Used to control user access and actions
    
- **RADIUS** vs **TACACS+**:
    
    - RADIUS combines authentication & authorization
        
    - TACACS+ separates them (more flexible)
        

### AAA Commands:

```plaintext
Router(config)#aaa new-model
Router(config)#aaa authentication login default local
Router(config)#username admin password cisco123
```

---

## 🚪 5.2 Access Control Lists (ACLs)

- Filters traffic based on criteria like IP, port, protocol
    
- Two types:
    
    - **Standard ACL**: Source IP only
        
    - **Extended ACL**: Source + destination + protocol/port
        

### Standard ACL:

```plaintext
Router(config)#access-list 10 permit 192.168.1.0 0.0.0.255
Router(config)#interface g0/0
Router(config-if)#ip access-group 10 in
```

### Extended ACL:

```plaintext
Router(config)#access-list 100 permit tcp any host 192.168.1.10 eq 80
Router(config)#interface g0/1
Router(config-if)#ip access-group 100 out
```

---

## 🛡️ 5.3 Device Hardening

- **Disable unused services and interfaces**
    
- **Strong passwords**
    
- **Enable SSH, disable Telnet**
    
- **Login banners**
    

### Example Config:

```plaintext
Router(config)#service password-encryption
Router(config)#banner motd ^Unauthorized Access Prohibited^
Router(config)#line vty 0 4
Router(config-line)#transport input ssh
Router(config)#username admin secret s3cr3t
```

---

## 🔐 5.4 SSH for Secure Remote Access

### SSH Setup:

```plaintext
Router(config)#ip domain-name ccna.local
Router(config)#crypto key generate rsa
Router(config)#ip ssh version 2
Router(config)#username admin password admin123
Router(config)#line vty 0 15
Router(config-line)#login local
Router(config-line)#transport input ssh
```

---

## 🧷 5.5 Switch Security

### Port Security:

```plaintext
Switch(config)#interface fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport port-security
Switch(config-if)#switchport port-security mac-address sticky
Switch(config-if)#switchport port-security maximum 1
Switch(config-if)#switchport port-security violation shutdown
```

### BPDU Guard:

```plaintext
Switch(config)#interface fa0/1
Switch(config-if)#spanning-tree bpduguard enable
```

---

## 🧪 5.6 Quick Labs

### Configure ACL:

```plaintext
Router(config)#access-list 101 deny tcp any any eq 23
Router(config)#access-list 101 permit ip any any
Router(config)#interface g0/0
Router(config-if)#ip access-group 101 in
```

### Enable AAA with local login:

```plaintext
Router(config)#aaa new-model
Router(config)#aaa authentication login default local
Router(config)#username netadmin password strongpass
```

---

## 📝 Review Questions

1. Which ACL type can filter by port?
    
    - A) Standard
        
    - B) Extended ✅
        
    - C) Named
        
    - D) Time-based
        
2. Which protocol separates auth & authorization?
    
    - A) RADIUS
        
    - B) TACACS+ ✅
        
    - C) LDAP
        
    - D) FTP
        
3. What command disables Telnet access?
    
    - A) no transport
        
    - B) transport input ssh ✅
        
    - C) line vty disable
        
    - D) crypto disable
        

---

## 🧠 Summary

- AAA controls access to devices
    
- ACLs restrict or permit traffic
    
- Device hardening improves overall security
    
- SSH ensures secure remote management
    
- Switch security prevents rogue device threats
    

---
