---
permalink: /labchallenges/
title: "Lab-Challenges"
---

# 🔬 **Labs & Exercises**

A collection of hands-on labs I completed to build practical networking, traffic analysis and web-request skills. Each entry includes **objective**, **tools**, **steps**, **findings**, **mitigation** and **lessons learned**.

---

## 1. **Examine TCP/IP and OSI Models in Action**
**Platform:** Local lab / Packet capture  
**Type:** Networking fundamentals / Protocol analysis  
**Date:** 2025-10-15

**Objective**  
Understand how protocols map between the **OSI** and **TCP/IP** models and observe live traffic to see which layer each protocol belongs to.

**Tools**  
- Wireshark  
- Packet Tracer (optional)  
- Terminal (`ping`, `traceroute`)

**Steps taken**
1. Created or captured simple network traffic (ping, HTTP, DNS, SSH).  
2. Opened capture in **Wireshark** and used protocol column + packet details.  
3. Mapped observed protocols to OSI/TCP-IP layers and applied filters (`ip`, `tcp`, `udp`, `http`, `dns`).

**Findings**
- TCP three-way handshake visible (SYN → SYN/ACK → ACK).  
- DNS queries use UDP (port 53).  
- MAC addresses and TTL values visible at Layer 2/3.

**Recommendations**
- Use protocol monitoring, network segmentation and logging for production networks.

**Lessons Learned**
- Visual packet inspection cements theory; Wireshark decoders are essential.

---

## 2. **Use Wireshark to Examine Network Traffic**
**Platform:** Wireshark / PCAP analysis  
**Type:** Packet analysis / Forensics

**Objective**  
Analyze PCAPs to identify suspicious traffic, extract artifacts (credentials, files) and summarize conclusions.

**Tools**  
- Wireshark / tshark  
- Text editor  
- base64/base32 decoders

**Steps taken**
1. Opened PCAP and ran `Statistics → Protocol Hierarchy`, `Conversations`, `Endpoints`.  
2. Applied filters: `http`, `dns`, `tcp.port==80`, `frame contains "Authorization"`.  
3. Used **Follow → TCP Stream** to reconstruct HTTP exchanges.  
4. Exported objects (`File → Export Objects → HTTP`) and used `tshark` for field extraction.

**Findings**
- Plaintext HTTP Basic auth headers in a stream.  
- Large DNS queries with long subdomains (possible tunneling).  
- Extracted file(s) with suspicious metadata.

**Mitigation**
- Enforce TLS/HTTPS, enable DNS filtering and logging, harden endpoints.

**Lessons Learned**
- `Follow TCP Stream` and protocol statistics speed up triage.

---

## 3. **Packet Tracer – Build a Switch & Router Network**
**Platform:** Cisco Packet Tracer  
**Type:** Network design / Configuration

**Objective**  
Design and configure a routed network with VLANs, inter-VLAN routing and verify host connectivity.

**Tools**  
- Cisco Packet Tracer

**Steps taken**
1. Built topology: two switches, one router, multiple hosts.  
2. Configured VLANs and assigned ports to VLANs.  
3. Configured router subinterfaces (router-on-a-stick) and assigned IPs.  
4. Tested connectivity using `ping` and `traceroute`; checked ARP and MAC tables.

**Findings**
- Inter-VLAN routing working after correct trunk/subinterface configuration.  
- Misconfigured trunk caused connectivity loss until corrected.

**Best Practices**
- Use SSH, strong credentials, VLAN pruning and port security.

**Lessons Learned**
- Packet Tracer is great for testing designs and learning subinterfaces.

---

## 4. **HTB Academy – Introduction to Network Traffic Analysis**
**Platform:** Hack The Box Academy  
**Type:** Guided course / Lab

**Objective**  
Learn capture collection, filtering, protocol identification and anomaly detection workflows.

**Tools**  
- Wireshark (guided)  
- Provided PCAPs and exercises

**Steps taken**
1. Completed modules on packet structure and common protocols.  
2. Practiced filters (`ip.addr==x.x.x.x`, `tcp.port==80`, `http.request`).  
3. Solved exercises identifying C2-like indicators and suspicious DNS.

**Findings**
- Better prioritization using statistics views and filter strategies.

**Recommendations**
- Implement flow logs, IDS rules (Suricata) and periodic PCAP reviews.

**Lessons Learned**
- Structured learning accelerates analyst workflow adoption.

---

## 5. **TryHackMe – DNS In Detail**
**Platform:** TryHackMe  
**Type:** Learning lab / CTF-style

**Objective**  
Understand DNS operations, record types, common attacks (spoofing, tunneling) and how to analyze DNS traffic.

**Tools**  
- dig / nslookup  
- Wireshark  
- Decoding tools (base32/base64)

**Steps taken**
1. Used `dig` to query A, AAAA, MX, TXT records.  
2. Captured DNS traffic and analyzed with `dns` filter in Wireshark.  
3. Identified long subdomain queries and decoded encoded payloads.

**Findings**
- Detected encoded payloads consistent with DNS tunneling.  
- Observed authoritative vs recursive resolver behaviors and TTL effects.

**Mitigation**
- Restrict recursion, implement DNSSEC where applicable, monitor outbound DNS.

**Lessons Learned**
- DNS is a common exfil channel; `dig` + Wireshark effectively surface issues.

---

## 6. **HTB Academy – Web Requests**
**Platform:** Hack The Box Academy  
**Type:** Web fundamentals / Security lab

**Objective**  
Explore HTTP request/response structure, headers, cookies and request manipulation for security testing.

**Tools**  
- Burp Suite (or browser dev tools)  
- cURL

**Steps taken**
1. Reviewed HTTP methods, status codes, and header usage.  
2. Intercepted and modified requests using Burp Suite.  
3. Tested for insecure headers and cookie flags; experimented with parameter tampering.

**Findings**
- Missing security headers (`X-Frame-Options`, `Content-Security-Policy`) on test endpoints.  
- Parameter tampering revealed input validation gaps.

**Mitigation**
- Add security headers, set cookie flags (`HttpOnly`, `Secure`, `SameSite`), validate inputs server-side and deploy WAF rules.

**Lessons Learned**
- Intercepting requests is crucial to understanding application behavior and vulnerabilities.

---
