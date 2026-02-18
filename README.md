# Fake CAPTCHA Multi-Payload Malware Campaign Analysis

A comprehensive technical analysis of a sophisticated 5-stage fileless malware campaign discovered targeting charity/volunteer organization payment pages.

**[View Full Report](https://sb21770.github.io/fakecaptcha-campaign-analysis/)

---

## Key Findings

| Attribute | Details |
|-----------|---------|
| **Campaign Status** | ACTIVE |
| **Attack Vector** | Fake CAPTCHA (Cloudflare impersonation) |
| **Execution Method** | Fileless (Memory-only via Donut shellcode) |
| **Origin** | Russian (excludes victims with 0x419 keyboard layout) |
| **Payloads** | Dual: Lumma Info Stealer + Marte Crypto Clipper |

## Attack Chain Overview

```
[Fake CAPTCHA] → [PowerShell Dropper] → [Shellcode Loader] → [Donut Shellcode] → [Final Payload]
     Stage 1           Stage 2              Stage 3              Stage 4           Stage 5
```

**Stage 5 is MODULAR** — C2 dynamically serves different payloads:
- **Lumma Stealer** (cs.bin) — Browser/wallet theft, Telegram exfiltration
- **Crypto Clipper** (clipx64.bin) — 17 hardcoded attacker wallets

## Quick IOCs

### Network Indicators
| IP Address | Role |
|------------|------|
| `158.94.209.33` | Stage 1 - Dropper |
| `178.16.53.70` | Stage 2 - Shellcode Loader |
| `94.154.35.115` | Payload Server |

### File Hashes (SHA256)
```
cptch.bin      : 35EC1234DA4E6433A797E5BA719E3A6B2CB455A8C3815601074B3FEF7BBC9B39
cptchbuild.bin : 23A240D9B928D7E35074D8C05CD5A8E6EDB0FFCC75A628CF7D5F6A952E2679B5
cs.bin         : 8A1DB4433E20B71698F63199B97C8A94DDA167CB24A169B0FC1092B1FB5E3CE0
clipx64.bin    : A609325C84A73341FEA831FFDB3E29C8D9C1619EB09669CF489ABDF9955B4DD6
```

### Behavioral Indicators
- **Mutex:** `CryptoClipboardMonitorMutex`
- **Russian Check:** `GetKeyboardLayout() == 0x419`

### Attacker Wallets (Partial)
```
Bitcoin:  113AiBfBwD16qFmPTRW54ATKyu6fcC7w3i
Ethereum: 0x1Ef52d5107493Cd358F77433Cf58dB3F737cA1d2
Solana:   7mnSBw4LXS8vz44LJuAzwdMxBSQrKjZNto5r7EJXnYQ1
```
*Full list of 17 wallets available in the report.*

## Report Sections

1. **Initial Discovery** — Fake CAPTCHA social engineering
2. **Payload Extraction** — Stage 1 & 2 PowerShell analysis
3. **VirusTotal Analysis** — Donut shellcode confirmation
4. **Dynamic Analysis** — Network behavior & payload discovery
5. **Memory Dump Analysis** — Stealer capabilities
6. **Fileless Execution** — .NET compilation bridge technique
7. **Crypto Clipper Analysis** — 17 wallets, live hijack verification
8. **Reverse Engineering** — Ghidra analysis (Russian check, clipboard swap)
9. **Complete Attack Chain** — Visual summary
10. **Live Campaign Verification** — C2 status confirmation
11. **IOCs** — Full indicator list
12. **MITRE ATT&CK Mapping**
13. **Conclusions & Recommendations**

## Tools Used

- **Dynamic Analysis:** Process Monitor, Wireshark, TCPView
- **Memory Analysis:** ProcDump, Strings
- **Static Analysis:** CyberChef, PE Studio, VirusTotal
- **Reverse Engineering:** Ghidra, x64dbg
- **Sandbox:** Local isolated VM

## MITRE ATT&CK Techniques

| ID | Technique |
|----|-----------|
| T1204.002 | User Execution: Malicious File |
| T1059.001 | PowerShell |
| T1055 | Process Injection |
| T1620 | Reflective Code Loading |
| T1115 | Clipboard Data |
| T1567 | Exfiltration Over Web Service |

## Disclaimer

This analysis was conducted for **educational and defensive purposes only**. All malware execution was performed in isolated virtual environments. The IOCs provided are intended to help organizations improve their security posture.

**Classification:** TLP:CLEAR

---

## Author

sb21770_Security Researcher  
Analysis Date: February 2026

