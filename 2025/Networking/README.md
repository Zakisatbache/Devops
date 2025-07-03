# Networking 

# Understand OSI & TCP/IP Models

# Purpose of OSI Model

>> Standardizes networking functions.

>> Helps troubleshoot network issues layer-by-layer.

>> Promotes interoperability between vendors and devices.

>> Breaks down complex networking into manageable parts.

# 7 Layers of the OSI Model (Top to Bottom)

| Layer No. | Layer Name       | Purpose / Function                                                                   |
| --------- | ---------------- | ------------------------------------------------------------------------------------ |
| 7         | **Application**  | End-user services, email, file transfer, web browsing (e.g., HTTP, FTP, SMTP).       |
| 6         | **Presentation** | Data translation, encryption, compression (e.g., converting formats like JPEG, MP4). |
| 5         | **Session**      | Manages sessions, controls dialogs, establishes & ends communication sessions.       |
| 4         | **Transport**    | Reliable data transfer, error checking (TCP/UDP), flow control.                      |
| 3         | **Network**      | Routing, logical addressing (e.g., IP), selects the best path for data.              |
| 2         | **Data Link**    | MAC addresses, error detection, frames, and access to physical media.                |
| 1         | **Physical**     | Transmits raw bits over the medium (cables, switches, electrical signals).           |

# Flow Diagram of OSI Model
![WHAT-IS-OSI-MODEL-7-LAYERS-EXPLAINED](https://github.com/user-attachments/assets/d0658e7f-4850-4855-a39e-36404b372ace)

# What is the TCP/IP Model?

The TCP/IP (Transmission Control Protocol/Internet Protocol) model is a four-layer conceptual framework used for designing and implementing network protocols. It is the foundation of the Internet and other modern networks.

Unlike the OSI model (which has 7 layers), the TCP/IP model has 4 layers, each responsible for specific networking tasks.

# TCP/IP Model Layers and Their Functions:

| Layer No. | Layer Name               | Description / Key Protocols                                                              |
| --------- | ------------------------ | ---------------------------------------------------------------------------------------- |
| 4         | **Application Layer**    | Includes protocols for end-user processes like HTTP, FTP, SMTP, DNS, etc.                |
| 3         | **Transport Layer**      | Provides communication between applications (TCP – reliable, UDP – fast but unreliable). |
| 2         | **Internet Layer**       | Handles logical addressing and routing (IP, ICMP, ARP).                                  |
| 1         | **Network Access Layer** | Manages physical transmission and data framing (Ethernet, Wi-Fi, MAC addresses).         |

# Comparison with OSI Model (for understanding):
| OSI Model        | TCP/IP Model   |
| ---------------- | -------------- |
| Application (7)  |                |
| Presentation (6) |                |
| Session (5)      | Application    |
| Transport (4)    | Transport      |
| Network (3)      | Internet       |
| Data Link (2)    |                |
| Physical (1)     | Network Access |


# Purpose of TCP/IP Model
>> Enables standardized communication over heterogeneous networks.
>> Forms the foundation of the Internet protocol suite.
>> Is simpler and more practical than OSI in real-world use.

# # Flow Diagram of TCP/IP Model
![image](https://github.com/user-attachments/assets/4a569d12-c3a3-4d73-907e-05c40f4b63fd)

# Protocols and Ports for DevOps:

# HTTP:

HTTP (HyperText Transfer Protocol) is the application layer protocol used for transmitting data (such as HTML files, images, videos) on the World Wide Web.

It is a request-response protocol between client (browser) and server.

Port: 80

# HTTPS:

HTTPS (HyperText Transfer Protocol Secure) is the secure version of HTTP, the protocol used for transferring data between a web browser (client) and a web server.

It ensures that all data exchanged between the client and server is encrypted and secure, protecting it from eavesdropping, tampering, or man-in-the-middle attacks.

Port: 443

# FTP:

FTP (File Transfer Protocol) is a standard network protocol used to transfer files between a client and a server over a TCP-based network, like the Internet.

It allows users to upload, download, rename, delete, and organize files on remote servers.

Port: 21 for control commands and port 20 for data transfer (in active mode).

# SMTP:

SMTP (Simple Mail Transfer Protocol) is a standard protocol used to send emails from an email client (like Outlook or Gmail) to an email server, and between email servers themselves.

Port: 25

# SSH:

SSH (Secure Shell) is a network protocol used to securely access and manage remote computers over an unsecured network. It enables users to log in to another computer, run commands, transfer files, and more — all encrypted to ensure privacy and security.

Port: 22

# DNS:

DNS (Domain Name System) is the "phonebook" of the internet. It translates human-readable domain names (like www.google.com) into IP addresses (like 142.250.182.132) that computers use to identify each other on the network.

Port: 53

# AWS EC2 and Security Groups

# How to Launch an EC2 Instance (Overview):
>> Log in to AWS Management Console
>> Go to EC2 Dashboard
>> Click “Launch Instance”
>> Choose an AMI (e.g., Ubuntu, Amazon Linux)
>> Choose an instance type (e.g., t2.micro)
>> Configure storage, networking, and security
>> Add key pair for SSH login
>> Launch the instance

# What are Security Groups in AWS EC2?
A Security Group is a set of firewall rules associated with EC2 instances that control network traffic at the instance level.

# Hands-On with Networking Commands:

>> ping
>> tracert
>> flushdns
>> nslookupp
>> net start
>> net stop
>> net stat
>> curl 
