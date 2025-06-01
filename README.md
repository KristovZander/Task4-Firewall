# Firewall Configuration and Testing - Task 4

This repository documents the process of configuring and testing basic firewall rules on a Windows system using Windows Defender Firewall with Advanced Security.

## Objective

The objective of this task was to learn how to configure and test basic firewall rules to allow or block traffic, specifically to block inbound traffic on a specific port (Telnet port 23) and then to test and verify this blocking. It also involved documenting the steps and understanding how firewalls filter network traffic.

## Tools Used

* **Operating System:** Windows 10/11
* **Firewall Tool:** Windows Defender Firewall with Advanced Security
* **Testing Tool:** Built-in Telnet Client (command line)

## Steps Performed and Documentation

### 1. Open Firewall Configuration Tool

* Accessed "Windows Defender Firewall with Advanced Security" via the Control Panel or Windows Search.
* The initial overview showed that Windows Defender Firewall is active for the Private Profile, with inbound connections not matching a rule being blocked.
    * **Screenshot:** [Windows Firewall Overview](Windows_Firewall_Overview.png)

### 2. List Current Firewall Rules

* Navigated to "Inbound Rules" to view the existing list of firewall rules.
    * **Screenshot:** [Windows Firewall Inbound Rules List](Windows_Firewall_Inbound_Rules_List.png)

### 3. Add a Rule to Block Inbound Traffic on Port 23 (Telnet)

* Created a new inbound rule using the "New Inbound Rule Wizard".
* Selected "Port" rule type.
* Specified "TCP" protocol and "Specific local ports" as `23` (for Telnet).
* Selected "Block the connection" as the action.
* Named the rule "Block telnet" for easy identification.
    * **Screenshots:**
        * [New Rule: Block Telnet Port (TCP 23)](New_Rule_Block_Telnet_Port_TCP_23.png)
        * [New Rule: Action - Block Connection](New_Rule_Action_Block_Connection.png)
        * [New Rule: Name - Block Telnet](New_Rule_Name_Block_Telnet.png)

### 4. Test the Rule (Attempt to connect to Port 23)

* The Telnet Client feature was enabled on the system to facilitate testing.
* **Command used to enable Telnet Client:**
    ```bash
    dism /online /Enable-Feature /FeatureName:TelnetClient
    ```
    * **Screenshot:** [Telnet Client Enabled and Block Test Failed](Telnet_Client_Enabled_and_Block_Test_Failed.png) (showing successful enabling)
* After enabling the Telnet Client, attempted to connect to localhost on port 23 to test the blocking rule.
* **Command used for testing:**
    ```bash
    telnet localhost 23
    ```
* **Result:** The connection attempt failed with "Could not open connection to the host, on port 23: Connect failed". This confirmed that the "Block telnet" firewall rule was successfully preventing inbound connections on port 23.
    * **Screenshot:** [Telnet Client Enabled and Block Test Failed](Telnet_Client_Enabled_and_Block_Test_Failed.png) (showing connection failure)

### 5. Add Rule to Allow SSH (Port 22)

* Created another new inbound rule using the "New Inbound Rule Wizard".
* Selected "Port" rule type.
* Specified "TCP" protocol and "Specific local ports" as `22` (for SSH).
* Selected "Allow the connection" as the action (implied, as it's the default for allowing).
* Named the rule "Allow ssh".
    * **Screenshots:**
        * [New Rule: Allow SSH Port (TCP 22)](New_Rule_Allow_SSH_Port_TCP_22.png)
        * [New Rule: Name - Allow SSH](New_Rule_Name_Allow_SSH.png)

### 6. Remove the Test Block Rule

* Located the "Block telnet" rule in the "Inbound Rules" list.
* Right-clicked on the rule and selected "Delete" to remove it, restoring the previous state.
    * **Screenshot:** [Delete Block Telnet Rule](Delete_Block_Telnet_Rule.png)

## How a Firewall Filters Traffic

A firewall acts as a security guard for a network, controlling incoming and outgoing network traffic based on a set of predefined security rules. It sits between a trusted internal network (like your PC) and an untrusted external network (like the internet).

Firewalls filter traffic by inspecting various aspects of network packets, including:
* **Source IP Address:** Where the traffic is coming from.
* **Destination IP Address:** Where the traffic is going.
* **Port Number:** The specific service or application the traffic is intended for (e.g., port 23 for Telnet, port 80 for HTTP, port 443 for HTTPS).
* **Protocol:** Whether it's TCP, UDP, ICMP, etc.
* **State:** Whether the traffic is part of an established, related, or new connection (for stateful firewalls).

By defining rules based on these criteria, a firewall can:
* **Block unwanted traffic:** Prevent access to specific services or from malicious sources.
* **Allow legitimate traffic:** Permit necessary communication for applications and services.
* **Log traffic:** Record connection attempts for auditing and security analysis.
* **Improve network security:** By preventing unauthorized access and blocking malicious traffic.

In this task, we specifically used an **inbound rule** to block traffic, meaning we controlled what traffic was *allowed to initiate a connection to our computer*.

## Interview Questions & Key Concepts

**Key Concepts:** Firewall configuration, network traffic filtering, ports, UFW, Windows Firewall, Telnet, SSH, Inbound/Outbound rules, Stateful/Stateless firewall, NAT.

**Potential Interview Questions (with brief answers):**

1.  **What is a firewall?**
    * A firewall is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules.
2.  **What is the difference between a stateful and stateless firewall?**
    * **Stateless Firewall:** Filters traffic based solely on individual packets and the rules defined, without considering the context or state of a connection.
    * **Stateful Firewall:** Tracks the state of active network connections. It allows return traffic for legitimate outgoing connections automatically, providing more secure and granular control.
3.  **What are inbound and outbound rules?**
    * **Inbound Rules:** Control traffic *coming into* your computer or network from an external source.
    * **Outbound Rules:** Control traffic *originating from* your computer or network and going to an external destination.
4.  **How does UFW simplify firewall management (even though you used Windows Firewall)?**
    * UFW (Uncomplicated Firewall) is a command-line interface for `iptables` on Linux, designed to simplify firewall configuration. It provides a more user-friendly syntax compared to complex `iptables` commands, making common firewall tasks easier.
5.  **Why block port 23 (Telnet)?**
    * Port 23 is used by the Telnet protocol, which transmits data, including login credentials, in plain text. Blocking it prevents unauthorized remote access and the interception of sensitive information due to its lack of encryption.
6.  **What are common firewall mistakes?**
    * Common mistakes include overly permissive rules, not regularly updating rules, not logging traffic, ignoring outbound traffic, and failing to test rules adequately.
7.  **How does a firewall improve network security?**
    * Firewalls improve network security by preventing unauthorized access, blocking malicious traffic (like malware, port scans), enforcing security policies, and limiting the attack surface by controlling which services are exposed to the network.
8.  **What is NAT in firewalls?**
    * NAT (Network Address Translation) is a method used by firewalls (and routers) to remap one IP address space into another. It typically allows multiple devices on a private network to share a single public IP address when connecting to the internet, conserving public IP addresses and adding a layer of security by hiding internal network topology.
