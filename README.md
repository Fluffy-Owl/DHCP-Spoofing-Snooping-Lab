# DHCP Spoofing and DHCP Snooping Lab

## Objective

The overall objective of this lab is to understand <ins>what happens in a DHCP spoofing incident</ins> and <ins>how we can prevent it through DHCP Snooping</ins>. Cisco's Packet Tracer will be used to simulate the DHCP spoofing attack and the DHCP snooping exercise.

**Objective 1:**

_DHCP Configuration:_ Configuring DHCP services on a server using GUI.

**Objective 2:**

*DHCP Spoofing:* Simulate an attacker trying to issue the following using a rogue DHCP server.
1. Fake IP addresses
2. Default gateway
3. DNS server 

**Objective 3:**

*DHCP Snooping:* Configure DHCP Snooping on the switch to prevent DHCP spoofing attacks. After configuring DHCP snooping, we will test it by `ipconfig /renew` on end hosts to see if the network is able to prevent DHCP spoofing.


**Bonus:**

We will be using a server to offer DHCP services to the network. However, at the very end, I will share how we can configure a router to offer DHCP services using CLI. In the example, we will also be able to offer fake domain name to the end hosts.


## Network Topology (Lab Setup)
[Picture here]
*Ref 1: Network Diagram*

**Devices Required:**

2 x Switch (Layer 2)
2 x Servers (1 legitimate DHCP server and 1 rogue DHCP server)
6 x end host (PCs and Laptops)
Optional:
1 x Router (ISP)
1 x Cloud (internet)

**Topology Overview:**

Connect all the end hosts and the Attacker PC to the switch0.
The switch0 is connected to switch1.
Switch1 is connceted to a DHCP server(which acts as the legitimate DHCP server) and the ISP.


## Skills Learned
[Bullet Points - Remove this afterwards]

- Familiarising to configure DHCP services on a server and a router for a network.
- Understanding how a rogue DHCP server can issue fake IP addresses, DNS server and default gateway.
- Configuring DHCP snooping and witnessing how DHCP snooping works.



## Steps

### Step 1: Configuring the DHCP server

1. After setting up the network topology shown above, proceed to the server and configure the DHCP services shown below.
_Services_ > _DHCP_
[Picture here]
*Ref 1: Network Diagram. Do remember to save and click the 'on' bullet to turn on the service.*

2. The same to be done to the rogue DHCP server.
[Picture here]
*Ref 1: Network Diagram. Do remember to save and click the 'on' bullet to turn on the service.*

3. Differentiate the Company's DHCP server and the attacker's DHCP server.
**Company's DHCP server:**
IP Address: Starting from 192.168.1.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
DNS server:8.8.8.8

**Attacker's DHCP server:**
IP Address: Starting from 192.168.1.20
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.22 (Attacker's Default Gateway)
DNS server:22.22.22.22 (Attacker's DNS server)

### Step 2: Simulate the attack

1. Simulate the end hosts requesting for IP address using DHCP protocol. This can be done through the GUI or the command prompt. We shall use the command prompt for this lab. Input `ipconfig /renew` in the command prompt as shown below.

[Picture here]
*Ref 1: Network Diagram*

[Picture here]
*Ref 1: Network Diagram*

**As shown, the attacker's DHCP server have manage to offer/issue the wrong IP address, default gateway and DNS server to the end hosts. The IP address issued starts from 192.168.1.20. The default gateway issued is 192.168.1.22 which is the attacker's IP address. And the DNS server issued is 22.22.22.22 instead of 8.8.8.8. This shows that the attaker's DHCP server manage to intercept the DHCP request messages from the end host and issuing the malicious IP address shown.**

### Step 3: DHCP Snooping
DHCP Snooping acts as a firewall between untrusted devices and the network's DHCP servers. It divide the ports into trusted and untrusted ports.
_Trusted Ports:_ These are typically the ports connected to legitimate DHCP servers. DHCP offers and acknowledgments are allowed to pass through these ports.
_Untrusted Ports:_ These are ports connected to end devices or any potential rogue DHCP servers. DHCP offers and acknowledgments from these ports are blocked to prevent unauthorized DHCP services.

1. Configure DHCP snooping on the switch. We configure port `F0/1` as the trusted port as the legitimate DHCP server offer messages will pass through this port.
[Picture here]
*Ref 1: Network Diagram*

### Step 4: Confirming that DHCP Snooping works
In this step, we will ensure that the key functions of DHCP Snooping works. The key functions of DHCP Snooping are:

_Monitoring DHCP Messages:_ The switch listens to DHCP messages exchanged between clients and DHCP servers.

_Filtering Unauthorized DHCP Responses:_ It ensures that only responses from trusted DHCP servers reach the clients.

_Building a DHCP Binding Table:_ The switch maintains a table mapping each client's MAC address to its IP address, lease time, VLAN, and port. This table is used to validate DHCP traffic and other security features like IP Source Guard and Dynamic ARP Inspection.

1. Renew all the IP address for the end hosts. To check that the IP address issued are legitimate, the IP address issued should be:

a. IP Address starts from 192.168.1.100
b. Default Gateway is 192.168.1.1
c. DNS server is 8.8.8.8
[Picture here]
*Ref 1: Network Diagram*
[Picture here]
*Ref 1: Network Diagram*
[Picture here]
*Ref 1: Network Diagram*
[Picture here]
*Ref 1: Network Diagram*

**As shown, the DHCP Snooping is successful. The IP address issued starts from 192.168.1.100. The default gateway issued is 192.168.1.1 which is the correct gateway. And the DNS server issued is 8.8.8.8. This shows that the DHCP Snooping is successful and the attacker is unable to issued malicious IP adresses to the end hosts.**

### Bonus Step: Configuring DHCP services using CLI (using routers)

![photo_2024-08-27_01-22-09](https://github.com/user-attachments/assets/13e37944-a306-4986-92ad-78dee5709a0c)

*Ref 1: How to configure DHCP services on a router using CLI*

![photo_2024-08-27_01-22-15](https://github.com/user-attachments/assets/5c221973-9812-470f-b5fc-7db6e534367e)

*Ref 1: An example (legitimate DHCP)*

![photo_2024-08-27_01-22-11](https://github.com/user-attachments/assets/1e563aba-0fa0-4778-bbb8-683250b40572)

*Ref 1: An example of how the attacker's DHCP issued the fake IP address and domain name*

## Benefits of DHCP Snooping

**Enhanced Network Security:** Protects against rogue DHCP servers and ensures that clients receive IP addresses from legitimate sources.

**Mitigation of Common Attacks:** Reduces the risk of Man-in-the-Middle (MitM), Denial of Service (DoS), and other attacks based on DHCP vulnerabilities.

**Improved Network Stability:** By preventing misconfiguration from rogue DHCP servers, DHCP Snooping helps maintain the overall stability of the network.

## Limitations of DHCP Snooping

**Complexity in Large Networks:** Managing DHCP Snooping across large and complex networks can be challenging, especially when multiple DHCP servers are involved.

**Overhead on Switches:** Monitoring and filtering DHCP traffic can introduce some processing overhead on switches, though this is usually minimal.

## Conclusion
By issuing a malicious DNS server IP address and malicious default gateway IP address poses significant security threats to a network and its users. These malicious configurations can be exploited by attackers to intercept, manipulate, and redirect network traffic for various nefarious purposes. The attacker can conduct MitM attacks and credential theft attacks, just to name a few.
Overall, DHCP Snooping is a critical tool in the network administrator's arsenal for maintaining secure and reliable network operations.
