# Standard Access Control List (ACL) Implementation

## 📌 Project Overview
This repository contains a Cisco Packet Tracer lab demonstrating the configuration and application of a Standard IP Access Control List (ACL). The project illustrates how to permit or deny traffic from specific hosts within a local area network from reaching a remote server, which is a fundamental security concept in the CCNA Essentials curriculum.

## 🏗️ Network Topology
The network consists of two main subnets connected via an ISR4331 Router. 

### Devices Used:
* **1x Router** (ISR4331)
* **1x Switch** (2960-24TT)
* **1x PC** (PC0)
* **1x Laptop** (Laptop0)
* **1x Server** (Server0)

### 📇 IP Addressing Table

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **PC0** | FastEthernet0 | 10.1.1.101 | 255.255.255.0 | 10.1.1.1 |
| **Laptop0** | FastEthernet0 | 10.1.1.100 | 255.255.255.0 | 10.1.1.1 |
| **Server0** | FastEthernet0 | 20.1.1.100 | 255.255.255.0 | 20.1.1.1 |
| **Router0** | gig0/0/0 | 10.1.1.1 | 255.255.255.0 | N/A |
| **Router0** | gig0/0/1 | 20.1.1.2 | 255.255.255.0 | N/A |

## 🎯 Lab Objective
The primary goal of this network design is to restrict specific network access using a Standard ACL:
1. **Deny** Laptop0 (`10.1.1.100`) from communicating with Server0.
2. **Permit** PC0 (`10.1.1.101`) and all other network traffic to communicate with Server0.

## ⚙️ Router Configuration
Below are the exact Cisco IOS commands used to configure the router interfaces and the Standard ACL.

### 1. Interface Configuration
Setting up the IP addresses and enabling the interfaces on Router0:
```console
Router> enable 
Router# configure terminal 

! Configuring Interface connected to the Switch (10.1.1.0/24 network)
Router(config)# interface gig0/0/0
Router(config-if)# ip address 10.1.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

! Configuring Interface connected to the Server (20.1.1.0/24 network)
Router(config)# interface gig0/0/1
Router(config-if)# ip address 20.1.1.2 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```

### 2. Standard ACL Configuration
Creating Access List `10` to deny the specific laptop host and permit everything else:
```console
Router(config)# access-list 10 deny host 10.1.1.100
Router(config)# access-list 10 permit any 
```

### 3. Applying the ACL
Applying the access list to the outbound traffic on the interface facing the server:
```console
Router(config)# interface gig0/0/1
Router(config-if)# ip access-group 10 out 
Router(config-if)# exit
Router(config)# exit 
Router# write memory
```

### 4. Verifying the ACL
To verify the access list has been created successfully:
```console
Router# show access-lists 
Standard IP access list 10
    10 deny host 10.1.1.100
    20 permit any
```

## 🧪 Testing & Verification
To ensure the network architecture and ACL are functioning correctly, the following ping tests are executed:

1. **From PC0 (Allowed Host):**
   * Open the Command Prompt on PC0.
   * Run: `ping 20.1.1.100`
   * **Expected Result:** Replies received successfully (Traffic permitted).

2. **From Laptop0 (Blocked Host):**
   * Open the Command Prompt on Laptop0.
   * Run: `ping 20.1.1.100`
   * **Expected Result:** Destination host unreachable (Traffic denied by ACL 10).

## 📚 References & Resources
* [Standard ACL Configuration Video Guide](https://youtu.be/eNKfpjY7oaE?si=mZE8YDIpWdOC-JkB)

