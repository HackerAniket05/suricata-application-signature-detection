# Suricata Application Signature Detection Lab

A hands-on cybersecurity project focused on network traffic analysis, TLS fingerprinting, and custom application signature detection using Suricata IDS/IPS.

## Project Overview

The objective of this project is to analyze encrypted network traffic, identify application-related TLS fingerprints, and develop custom Suricata rules for detection.

The lab uses AnyDesk traffic as a case study.

## Tools and Technologies

- Ubuntu 22.04
- Suricata 6.0.4
- tcpdump
- tshark
- JA3 TLS fingerprinting
- iptables
- NFQUEUE

## Project Workflow

1. Capture network traffic using tcpdump.
2. Analyze packet captures using tshark.
3. Inspect TLS ClientHello messages.
4. Extract JA3 fingerprints.
5. Develop custom Suricata signatures.
6. Validate detection rules.
7. Test signatures against offline PCAP files.
8. Configure NFQUEUE for inline traffic inspection.
9. Review detection alerts and IPS statistics.

## Application Fingerprint

Observed JA3 fingerprint:

`29b5a018fa5992fe23560c16af0dc9fc`

This fingerprint was observed in the analyzed AnyDesk traffic.

## Detection Rule

```text
alert tls any any -> any any (msg:"LOCAL AnyDesk JA3 fingerprint detected"; ja3.hash; content:"29b5a018fa5992fe23560c16af0dc9fc"; sid:1000001; rev:2;)
```

## Validation Results

| Test | Result |
|---|---|
| Traffic capture | Successful |
| TLS analysis | Successful |
| JA3 extraction | Successful |
| Rule validation | Successful |
| Offline PCAP detection | Successful |
| NFQUEUE initialization | Successful |
| Live IPS packet drops | Observed |
| AnyDesk-specific live blocking | Observed |

## Learning Outcomes

- Network traffic analysis
- TLS handshake investigation
- JA3 fingerprinting
- Custom IDS rule development
- Suricata configuration and troubleshooting
- Linux firewall and NFQUEUE
- Offline PCAP investigation

## Limitations

JA3 fingerprints are not unique application identifiers. Fingerprints can change between application versions or TLS implementations.

Although IPS packet drops were observed, they were not conclusively attributed to the custom AnyDesk signature.

## Disclaimer

This project was developed in a personal laboratory environment for educational and defensive cybersecurity research.
************************************************************************************************Aniket Jha ********************************************************************************************************************************************
