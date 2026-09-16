# Task 1 — Foundational Cybersecurity Assessment

A complete security assessment of a small fictional organization, covering asset management, threat and vulnerability analysis, the CIA triad, network architecture, the OSI/TCP-IP models, core protocols, secure communication and endpoint hardening.

![Status](https://img.shields.io/badge/status-complete-success)
![Type](https://img.shields.io/badge/type-academic%20assessment-blue)
![Scope](https://img.shields.io/badge/testing-none%20performed-lightgrey)

---

## About this assessment

**Organization assessed:** Northwind Craft Supplies Pvt. Ltd. — a **fictional** 25-person business with employee laptops, an on-premises file server, a router/firewall, a managed switch, Wi-Fi, a public web service with a customer order portal, and a single ISP connection.

**Role:** Junior cybersecurity analyst conducting a foundational assessment.

**Deliverable:** a structured set of documents identifying security weaknesses and recommending practical, prioritised controls appropriate to a small business with limited budget and no dedicated security staff.

---

## Key findings at a glance

| Metric | Result |
|---|---|
| Assets inventoried | **12** |
| Threats identified | **14** |
| Vulnerabilities identified | **16** |
| Risks registered | **13** — 3 Critical, 6 High, 4 Medium |
| Controls recommended | **15** across endpoint, network, account, data and governance |
| Endpoint hardening checks | **80** |

**The three Critical risks**

1. **R-01** — Ransomware encrypts the file server *and* the writable, co-located backup NAS, leaving no recovery path.
2. **R-02** — SQL injection in the order portal exposes ~8,000 customer records, triggering DPDP Act 2023 notification duties.
3. **R-03** — Phishing captures Microsoft 365 credentials with no MFA in place, enabling invoice fraud.

**The central conclusion:** most of the identified risk can be eliminated at effectively zero cost. Enabling MFA, closing an exposed firewall admin page, removing local admin rights, patching, isolating the backup target and fixing input validation are configuration and discipline changes — not purchases.

---

## Repository structure

```
.
├── README.md                                   ← you are here
├── docs/
│   ├── 01-asset-inventory.md                   Asset inventory and classification
│   ├── 02-threat-vulnerability-assessment.md   Threats, vulnerabilities, definitions, risk register
│   ├── 03-cia-triad-assessment.md              CIA triad impact assessment
│   ├── 04-network-architecture.md              Current and target network design, IP plan, OSI mapping
│   ├── 05-protocols-and-secure-communication.md  IP, DNS, DHCP, TCP, UDP, HTTP, HTTPS/TLS
│   ├── 06-security-controls.md                 15 recommended controls + remediation roadmap
│   ├── 07-endpoint-hardening-checklist.md      80-item hardening checklist
│   └── 08-final-report.md                      ★ Full professional report (5–8 pages)
└── diagrams/
    └── network-diagram.svg                     Standalone target-state architecture diagram
```

---

## Deliverables map

Every required deliverable and where to find it:

| Required deliverable | Document |
|---|---|
| Asset inventory and classification table | [`docs/01-asset-inventory.md`](docs/01-asset-inventory.md) |
| Threat and vulnerability assessment table | [`docs/02-threat-vulnerability-assessment.md`](docs/02-threat-vulnerability-assessment.md) |
| Threat vs vulnerability vs risk vs control | [`docs/02`](docs/02-threat-vulnerability-assessment.md#21-core-definitions-threat-vulnerability-risk-control) §2.1 |
| CIA triad impact assessment | [`docs/03-cia-triad-assessment.md`](docs/03-cia-triad-assessment.md) |
| Network architecture diagram | [`docs/04`](docs/04-network-architecture.md) + [`diagrams/network-diagram.svg`](diagrams/network-diagram.svg) |
| Protocol and secure-communication explanation | [`docs/05-protocols-and-secure-communication.md`](docs/05-protocols-and-secure-communication.md) |
| 10+ security controls (endpoint, network, account, data) | [`docs/06-security-controls.md`](docs/06-security-controls.md) |
| Endpoint-hardening checklist | [`docs/07-endpoint-hardening-checklist.md`](docs/07-endpoint-hardening-checklist.md) |
| 5–8 page professional report | [`docs/08-final-report.md`](docs/08-final-report.md) |

**Start with [`docs/08-final-report.md`](docs/08-final-report.md)** — it summarises everything and links to the detailed documents.

---

## Frameworks and standards referenced

- **NIST Cybersecurity Framework 2.0** — Govern, Identify, Protect, Detect, Respond, Recover
- **CIS Critical Security Controls v8**, Implementation Group 1 (small-organization baseline)
- **ISO/IEC 27001:2022** — information security management
- **STRIDE** — threat modelling (Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege)
- **OWASP Top 10** — web application security risks
- **NIST SP 800-63B** — modern authentication and password guidance
- **ISO/IEC 7498-1** — the OSI seven-layer reference model

---

## Ethics and scope statement

This is a **documentation and analysis exercise**. It is important to be explicit about what was and was not done.

**What was done**
- Analysis of a fictional environment constructed for this assessment
- Threat modelling and vulnerability identification through conceptual configuration review
- Recommendation of defensive controls based on published standards

**What was not done**
- ❌ No scanning, probing or enumeration of any live system
- ❌ No exploitation of any vulnerability
- ❌ No credential testing, brute forcing or password attacks
- ❌ No phishing or social engineering, simulated or otherwise
- ❌ No malware handling or deployment
- ❌ No denial-of-service testing
- ❌ No collection of personal data from any real individual

All IP addresses use IETF documentation ranges (RFC 1918, RFC 5737) and all domain names use the reserved `.example` TLD, so nothing in this repository references a real host.

Where offensive techniques are described — SQL injection, DHCP spoofing, DNS hijacking, amplification attacks — they are explained at a conceptual level to justify the corresponding defensive control. No operational or exploitation detail is included.

Any future practical work associated with this task will be performed exclusively on intentionally vulnerable training environments (such as DVWA, OWASP Juice Shop or Metasploitable), published CTF platforms (TryHackMe, Hack The Box), or self-built laboratory systems on personally owned hardware.

---

## What I learned

- **Precision in terminology drives better decisions.** Threat, vulnerability, risk and control are not synonyms. You cannot patch a threat, and treating the absence of past incidents as evidence of security is a category error.
- **Architecture is a security control.** The most impactful recommendation in this assessment is not a product — it is VLAN segmentation and inverting the backup from push to pull. Both are free.
- **Identity is the highest-leverage layer.** MFA reduces three of thirteen registered risks, costs nothing on Microsoft 365, and takes under an hour to enable.
- **An unverified backup is an assumption, not a control.** Northwind has backups and no evidence they can be restored. Those are very different things.
- **Layered thinking beats perimeter thinking.** TLS at Layer 6 does nothing about an unlocked server cupboard at Layer 1. Controls have to span the whole stack.

---

## License

Released under the MIT License for educational use. See [`LICENSE`](LICENSE).

---

*Prepared as an academic assessment. The organization, systems, addresses and findings described are fictional and constructed for learning purposes.*
