# ja3-ja4-tls-fingerprinting

# JA3/JA4 TLS Fingerprinting: Distinguishing Malware C2 from Legitimate VPN Traffic

A research project investigating how JA3 and JA4 TLS client fingerprints can be used to differentiate malicious command-and-control (C2) traffic from legitimate VPN sessions in encrypted network captures.

> Coursework completed for **CGS 4285 — Forensic Computing**

---

## Overview

Modern malware increasingly uses TLS-encrypted channels to hide command-and-control communication, often blending in with legitimate VPN or HTTPS traffic. Because the payload is encrypted, defenders need other signals to identify malicious activity. **JA3** and its successor **JA4** generate fingerprints from the unencrypted portion of the TLS handshake — making it possible to identify the *client software* without decrypting the traffic.

This project explores whether JA3/JA4 fingerprints alone are reliable enough to distinguish malware C2 sessions from legitimate VPN sessions, and where that approach breaks down.

---

## Background

**JA3** (developed by Salesforce, 2017) creates an MD5 hash from five fields in the TLS Client Hello: version, accepted ciphers, extensions, elliptic curves, and elliptic curve formats. Same client software → same hash.

**JA4** (developed by FoxIO, 2023) is the next-generation fingerprint. Unlike JA3, JA4 is human-readable, includes ALPN and SNI signals, and is broken into segments (`JA4_a`, `JA4_b`, `JA4_c`) that can be matched independently. JA4 was designed specifically to address evasion techniques that defeat JA3.

---

## Research Question

> Can JA3/JA4 fingerprints reliably distinguish malware C2 traffic from legitimate VPN traffic in real-world packet captures, and what evasion techniques degrade that reliability?

---

## Methodology

1. **Sample collection** — gathered packet captures (.pcap) from:
   - Known malware families with TLS-based C2 *(list specific families: e.g., Cobalt Strike, TrickBot, etc.)*
   - Legitimate VPN clients *(list: e.g., OpenVPN, WireGuard, commercial providers)*
2. **Fingerprint extraction** — generated JA3 and JA4 hashes from each session using `tshark` / Wireshark with the JA4 plugin.
3. **Comparison** — compared fingerprint distributions across malicious and legitimate categories.
4. **Evasion analysis** — examined cases where malware mimics common client fingerprints to evade detection.

---

## Key Findings

*Replace with your actual results from the paper:*

- *Finding 1 — e.g., "JA3 alone misclassified X% of [family] sessions as legitimate VPN traffic due to shared TLS libraries."*
- *Finding 2 — e.g., "JA4's segmented structure caught Y% more variants than JA3 in the same dataset."*
- *Finding 3 — e.g., "Both fingerprints failed against malware using browser-impersonation libraries like utls."*
- *Finding 4 — e.g., "Combining JA4 with destination-IP reputation reduced false positives by Z%."*

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| Wireshark / tshark | Packet capture and TLS handshake inspection |
| JA4 plugin (FoxIO) | Generating JA4 fingerprints from pcap files |
| Python | Parsing, hashing, and statistical comparison |
| Sample pcaps | Malware traffic from public repositories (Malware-Traffic-Analysis.net), VPN traffic captured in lab |

---

## Repository Contents

```
.
├── README.md                  # This file
├── paper/
│   └── ja3-ja4-research.pdf   # Full research paper
├── video/
│   └── tutorial-link.md       # Link to the walkthrough video
├── captures/                  # Sanitized sample pcaps (if shareable)
├── analysis/                  # Scripts used for fingerprint extraction
└── references.md              # Cited sources
```

---

## Video Walkthrough

A companion video tutorial walks through the methodology and demonstrates fingerprint extraction in Wireshark.

> 🎥 **[Watch the walkthrough →](LINK_TO_YOUR_VIDEO)**

---

## What I Learned

- Practical use of TLS handshake metadata for traffic classification without decryption.
- Tradeoffs between JA3 (broad adoption, easier to evade) and JA4 (more granular, newer ecosystem).
- How threat actors adapt — including the use of randomized cipher suites and `utls` libraries — to defeat fingerprint-based detection.
- The role of TLS fingerprinting as one signal in a layered detection strategy, not a silver bullet.

---

## References

*Add the sources cited in your paper. Examples:*

- Althouse, J. (2017). *TLS Fingerprinting with JA3 and JA3S.* Salesforce Engineering.
- FoxIO. (2023). *JA4+ Network Fingerprinting.*
- Anderson, B., & McGrew, D. (2017). *Identifying Encrypted Malware Traffic with Contextual Flow Data.*

---

## Author

**Edwin Miranda**
Forensic Computing Student | CGS 4285
🔗 [LinkedIn](LINK) · [GitHub](LINK)

---

*This project was completed as academic coursework. Pcap samples involving real malware are referenced from public threat-research repositories; no live malware is hosted in this repo.*
