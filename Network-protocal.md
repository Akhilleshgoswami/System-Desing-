
# Networking Protocols – How Data Travels Between Two Devices

## Why Do We Need Networking Protocols?

When two devices communicate over the internet, they must follow a set of rules.

These rules define:

- How devices connect  
- How data is formatted  
- How data is transmitted  
- How reliability is ensured  
- How security is maintained  

These rules are called **Networking Protocols**.

Without protocols:
- Devices cannot find each other  
- Data may get lost  
- Communication will be unreliable  

---

# OSI Model (Focused on 3 Important Layers)

Although OSI has 7 layers, here we focus on:

1. Network Layer  
2. Transport Layer  
3. Application Layer  

Each layer has a specific responsibility.

---

# 1️⃣ Network Layer

## Responsibility  
Find the destination device on the network.

## Protocols  
- IPv4  
- IPv6  

## What It Does  
- Assigns IP addresses  
- Helps routers forward packets  
- Identifies source and destination  

### Real Example (WhatsApp / Telegram / Instagram)

Let’s say you are using WhatsApp and want to send a message to your friend.

First task:  
We must find your friend’s device on the network.

How?

- Every device connected to the internet has an IP address.
- IPv4 / IPv6 help in identifying that device.
- Routers use these IP addresses to route the packet to the correct destination.

Network layer answers:  
**Where should this data go?**

---

# 2️⃣ Transport Layer

## Responsibility  
Ensure reliable and proper delivery of data between client and server.

## Protocols  
- TCP  
- UDP  

Transport layer answers:  
**How should this data travel?**

---

## TCP (Transmission Control Protocol)

### Features
- Connection-oriented  
- Reliable  
- Ordered delivery  
- Guaranteed delivery  

### Three-Way Handshake (Connection Setup)

Before sending data:

1. Client → SYN  
2. Server → SYN-ACK  
3. Client → ACK  

Now both sides agree:
- How they will communicate  
- Sequence numbers  
- Protocol rules  

Connection is established.

### How TCP Sends Data

- Data is divided into packets.
- Each packet has a sequence number.
- Server sends acknowledgment (ACK) for each packet.
- If any packet is lost → it is retransmitted.
- Delivery is guaranteed and in order.

### Example (Messaging App)

When you send a message on WhatsApp:

1. Message is broken into packets.
2. Packets are sent to WhatsApp server.
3. Server sends acknowledgment.
4. If any packet fails → it is resent.
5. Once received, server forwards it to your friend.

Here reliability is important because:
Message delivery must be guaranteed.

---

## UDP (User Datagram Protocol)

### Features
- Connectionless  
- No handshake  
- No guaranteed delivery  
- No ordering  
- Faster than TCP  

### Example

Used in:
- Video calls  
- Online gaming  
- Live streaming  

In a video call:
If one frame is lost → it is ignored.
Speed matters more than perfect delivery.

UDP does not:
- Maintain order  
- Retransmit lost packets  

That’s why it is faster.

---

# 3️⃣ Application Layer

## Responsibility  
Define how applications communicate.

It decides:
- Data format  
- Request & response structure  
- Encryption rules  

## Protocols  
- HTTP  
- HTTPS  
- FTP  
- SMTP  
- WebSocket  
- WebRTC  

Application layer answers:  
**How should the message be structured and delivered?**

---

# Complete Example: Sending Message on WhatsApp

Let’s understand full flow step-by-step:

Step 1 – Network Layer  
IPv4 / IPv6 finds your friend’s device through routing.

Step 2 – Transport Layer  
TCP establishes a reliable connection using 3-way handshake.

Step 3 – Application Layer  
WhatsApp uses HTTPS or WebSocket:
- Message is encrypted.
- Sent to WhatsApp server.
- Server forwards message to your friend.

Flow:

Your Device → WhatsApp Server → Friend’s Device

Most apps like:
- WhatsApp  
- Telegram  
- Instagram  

Use Client-Server model.

Client:
Initiates connection.

Server:
Processes request and forwards data.

---

# HTTPS vs HTTP

HTTP:
- Not encrypted  

HTTPS:
- Encrypted  
- Secure communication  
- Used in login systems & banking apps  

---

# WebSocket

- Full-duplex communication  
- Persistent connection  
- Client and server can send messages independently  

Used in:
- Chat apps  
- Live notifications  
- Real-time systems  

---

# Summary

Network Layer  
→ Finds the device (IP address, Routing)

Transport Layer  
→ Ensures reliable or fast delivery (TCP / UDP)

Application Layer  
→ Defines how applications communicate (HTTP, WebSocket, etc.)

Together, these layers make modern apps like WhatsApp, Telegram, and Instagram work smoothly.

