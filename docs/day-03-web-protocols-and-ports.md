# Day 3 — Web Protocols and Ports

**Syllabus:** Investigate and describe the function of web protocols and their ports — HTTP, HTTPS, TCP/IP, DNS, FTP, SFTP, SSL, TLS, SMTP, POP3, IMAP
**Outcomes:** SE-12-03
**Textbook:** Section 11.2

---

## Key Terms

**Protocol** — A set of rules that defines how data is formatted, transmitted, and received between devices. Protocols ensure that different systems can communicate reliably.

**Port** — A virtual communication endpoint on a device, identified by a number (0–65535). Ports allow a single device to run multiple network services simultaneously. Think of a port like a specific door into a building — the building is the IP address, the door is the port.

**TCP (Transmission Control Protocol)** — A connection-oriented protocol that guarantees packet delivery, order, and error checking. Slower but reliable. Used for web browsing, email, file transfer.

**UDP (User Datagram Protocol)** — A connectionless protocol with no delivery guarantee. Faster than TCP. Used for video streaming, online gaming, and DNS lookups where speed matters more than perfect accuracy.

**IP (Internet Protocol)** — Responsible for addressing and routing packets across networks. Works with TCP as the **TCP/IP** suite.

**HTTP (Hypertext Transfer Protocol)** — The protocol used to transfer web pages from server to client. Data is sent in plaintext — not encrypted. Port 80.

**HTTPS (HTTP Secure)** — HTTP with encryption via TLS. All data between client and server is encrypted. Port 443. Indicated by a padlock icon in the browser.

**FTP (File Transfer Protocol)** — Used to transfer files between a client and server. Not encrypted — credentials and data are sent in plaintext. Port 21.

**SFTP (SSH File Transfer Protocol)** — Secure version of FTP. Encrypts data using SSH. Port 22.

**SSL (Secure Sockets Layer)** — An older encryption protocol, now deprecated. Replaced by TLS. The term "SSL" is still commonly used colloquially to refer to TLS.

**TLS (Transport Layer Security)** — The current standard encryption protocol used by HTTPS and other secure services. Provides authentication, confidentiality, and integrity.

**SMTP (Simple Mail Transfer Protocol)** — Used to send email from a client to a mail server, and between mail servers. Port 25 (server-to-server) or 587 (client-to-server with authentication).

**POP3 (Post Office Protocol 3)** — Used to retrieve email from a mail server. Downloads emails to the local device and typically deletes them from the server. Port 110 (plain) / 995 (encrypted).

**IMAP (Internet Message Access Protocol)** — Used to access email stored on a mail server. Emails remain on the server and can be accessed from multiple devices. Port 143 (plain) / 993 (encrypted).

**DNS (Domain Name System)** — Translates domain names to IP addresses. Port 53.

---

## Theory

### Why Protocols and Ports Matter

Every time your computer communicates over the internet, it uses a protocol. Without agreed-upon rules, different systems could not interoperate. Protocols define:
- How data is structured
- How connections are established and terminated
- How errors are detected and handled

Ports allow a single server to handle many different services simultaneously. A web server might run HTTP (port 80), HTTPS (port 443), and FTP (port 21) all at once.

### TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (handshake) | Connectionless |
| Delivery guarantee | Yes — retransmits lost packets | No |
| Order guarantee | Yes | No |
| Speed | Slower | Faster |
| Use cases | Web, email, file transfer | Video streaming, gaming, DNS |

**TCP three-way handshake:**
1. Client sends **SYN** (synchronise) — "I want to connect"
2. Server responds **SYN-ACK** (synchronise-acknowledge) — "Acknowledged, ready"
3. Client sends **ACK** (acknowledge) — "Connected"

Connection is now established and data transfer begins.

### HTTP vs HTTPS

HTTP sends data as readable plaintext. Anyone intercepting the network traffic can read the data — including passwords and credit card numbers.

HTTPS wraps HTTP in TLS encryption. The data is encrypted before transmission and decrypted only by the intended recipient.

```
HTTP request (visible to anyone):
GET /login HTTP/1.1
Host: bank.com
username=dean&password=mypassword123

HTTPS request (encrypted, unreadable in transit):
[encrypted gibberish]
```

All modern browsers flag HTTP sites as "Not secure". HTTPS is now the standard.

### Email Protocols

Email uses three different protocols for different tasks:

| Protocol | Purpose | Direction |
|---|---|---|
| SMTP | Sending email | Client → Server, Server → Server |
| POP3 | Retrieving email (downloads, deletes from server) | Server → Client |
| IMAP | Accessing email (stays on server) | Server ↔ Client |

**IMAP vs POP3:** If you check email on multiple devices (phone, laptop, desktop), use IMAP — emails stay on the server and sync across all devices. POP3 downloads and removes from the server, so emails only appear on one device.

### Protocol and Port Reference Table

| Protocol | Port | Encrypted | Purpose |
|---|---|---|---|
| HTTP | 80 | No | Web pages |
| HTTPS | 443 | Yes (TLS) | Secure web pages |
| FTP | 21 | No | File transfer |
| SFTP | 22 | Yes (SSH) | Secure file transfer |
| SMTP | 25 / 587 | Optional | Sending email |
| POP3 | 110 / 995 | Optional / Yes | Receiving email (download) |
| IMAP | 143 / 993 | Optional / Yes | Receiving email (server-side) |
| DNS | 53 | No (typically) | Domain name resolution |
| SSH | 22 | Yes | Secure remote terminal |

---

## Objective Response Questions

**1.** A user is accessing their company email from three different devices. Which email retrieval protocol is most appropriate?

- A) SMTP
- B) POP3
- C) IMAP
- D) FTP

**Answer: C** — IMAP keeps emails on the server, allowing synchronised access from multiple devices.

---

**2.** Which of the following correctly describes the purpose of a port number?

- A) It identifies which physical network cable a device is using
- B) It identifies a specific communication endpoint on a device, allowing multiple services to run simultaneously
- C) It stores the IP address of a remote server
- D) It encrypts data before transmission

**Answer: B**

---

**3.** A developer notices their web application is running on HTTP rather than HTTPS. Which of the following is the most significant security risk?

- A) The website will load more slowly
- B) Users cannot bookmark the website
- C) Data transmitted between the client and server, including passwords, is sent in plaintext
- D) The server will reject connections from mobile devices

**Answer: C**

---

## Try It

1. Open your browser's DevTools (F12) and go to the **Network** tab.
2. Load any website and click on one of the requests.
3. Find and record:
   - The protocol used (HTTP or HTTPS)
   - The request method (GET, POST, etc.)
   - The status code returned (e.g. 200, 404)
4. Visit a site that still uses HTTP (try `http://neverssl.com`) and observe what your browser displays.
5. Create a table in your notes listing five protocols from today's lesson, their ports, whether they are encrypted, and their purpose. Do this from memory first, then check.
