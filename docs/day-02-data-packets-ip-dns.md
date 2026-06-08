---
title: Day 2 — Data Packets, IP and DNS
parent: Week 1 — Web Concepts and Standards
nav_order: 2
---

# Day 2 — Data Packets, IP Addresses and DNS

**Syllabus:** Investigate and practise how data is transferred on the internet — data packets, IP addresses (IPv4), domain name systems (DNS)
**Outcomes:** SE-12-03
**Textbook:** Section 11.2

---

## Key Terms

**Data packet** — A small unit of data transmitted over a network. Large files are broken into many packets, sent independently, and reassembled at the destination. Each packet contains a header (source, destination, sequence number) and a payload (the actual data).

**IP address** — A unique numerical label assigned to every device on a network. Used to identify the source and destination of packets. Example: `192.168.1.1`

**IPv4** — Internet Protocol version 4. Uses 32-bit addresses written as four numbers separated by dots. Each number ranges from 0–255. Example: `203.32.100.5`. Supports approximately 4.3 billion unique addresses.

**IPv6** — Internet Protocol version 6. Uses 128-bit addresses to solve the IPv4 address exhaustion problem. Example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`. Supports 340 undecillion addresses.

**Domain Name System (DNS)** — A distributed database that translates human-readable domain names (e.g. `google.com`) into IP addresses (e.g. `142.250.70.46`). DNS is often described as the phone book of the internet.

**Router** — A networking device that forwards data packets between networks, directing them toward their destination.

**Bandwidth** — The maximum amount of data that can be transmitted over a network connection in a given time, typically measured in Mbps (megabits per second).

**Latency** — The time delay between a packet being sent and received, measured in milliseconds (ms). High latency causes lag.

---

## Theory

### How Data Travels Across the Internet

When you load a webpage, the following happens:

1. Your browser sends a **request** to the web server
2. The request is broken into **packets**
3. Each packet travels independently across the network, potentially via different routes
4. Packets arrive at the destination and are **reassembled** in order
5. The server sends a **response** (the webpage) back, also as packets

This process is governed by **TCP/IP** — the suite of protocols that defines how packets are addressed, sent, and received.

### Data Packets in Detail

Each packet has two parts:

**Header** — contains metadata:
- Source IP address
- Destination IP address
- Packet sequence number (so packets can be reassembled in order)
- Total number of packets
- Error-checking data (checksum)

**Payload** — the actual data being transmitted (a fragment of the file or message)

If a packet is lost or corrupted in transit, TCP requests it to be resent. This is why downloads sometimes slow down but rarely fail completely.

### IPv4 Addresses

IPv4 addresses use **dotted decimal notation**: four groups of numbers separated by dots.

```
203 . 32 . 100 . 5
 |      |     |    |
 8-bit  8-bit 8-bit 8-bit  = 32 bits total
```

Each group ranges from 0 to 255 (256 possible values = 2⁸).

**Special addresses:**
- `127.0.0.1` — loopback address, refers to your own machine (localhost)
- `192.168.x.x` — private network range (local network, not routable on the public internet)

IPv4 can only support ~4.3 billion unique addresses. With billions of devices online, IPv4 addresses are exhausted — which is why IPv6 was developed.

### DNS — The Internet's Phone Book

You type `www.google.com`. Your computer needs an IP address to actually connect. DNS resolves this:

```
You type:       www.google.com
DNS returns:    142.250.70.46
Browser uses:   142.250.70.46 to connect
```

**DNS resolution steps:**
1. Browser checks its local cache — has it visited this domain before?
2. If not, asks the **recursive resolver** (usually your ISP)
3. Resolver queries a **root name server**
4. Root server directs to the **TLD server** (e.g. `.com` server)
5. TLD server directs to the **authoritative name server** for `google.com`
6. Authoritative server returns the IP address
7. Browser caches the result and connects

This entire process typically takes under 50ms.

### Traceroute

`traceroute` (Linux/Mac) or `tracert` (Windows) shows the path packets take to reach a destination, listing every router (hop) along the way.

```bash
traceroute google.com
```

Sample output:
```
 1  192.168.1.1       1ms   (your home router)
 2  10.0.0.1          5ms   (ISP gateway)
 3  203.50.12.1       8ms   (ISP backbone)
 4  142.250.70.46    15ms   (Google server)
```

Each line is a **hop** — a router that forwarded your packet.

---

## Objective Response Questions

**1.** A 10MB file is sent over the internet. Which of the following best describes how it is transmitted?

- A) The file is sent as one continuous stream of data
- B) The file is broken into packets, each sent independently and reassembled at the destination
- C) The file waits until a direct connection is established before transmission begins
- D) The file is compressed into a single packet and sent at once

**Answer: B**

---

**2.** A device has the IP address `192.168.0.14`. What does this indicate?

- A) The device is a web server accessible from the internet
- B) The device is on a private local network
- C) The device is using IPv6
- D) The device is the DNS resolver for the network

**Answer: B** — `192.168.x.x` is a private IP range, not routable on the public internet.

---

**3.** When a user types `www.school.nsw.edu.au` into a browser, which system is responsible for converting this into an IP address?

- A) HTTP
- B) FTP
- C) DNS
- D) TCP

**Answer: C**

---

## Try It

1. Open a terminal (Command Prompt on Windows, Terminal on Mac/Linux).
2. Run the following and record the IP address returned:
   ```bash
   nslookup google.com
   ```
3. Run a traceroute to the same address:
   ```bash
   traceroute google.com    # Mac/Linux
   tracert google.com       # Windows
   ```
4. Count the number of hops. Look up where one of the intermediate IP addresses is located using [iplocation.net](https://www.iplocation.net).
5. Write a brief explanation (3–4 sentences) of what the traceroute output tells you about the journey your data takes.
