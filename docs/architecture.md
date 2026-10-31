# Network Architecture

## 1. Overview

This project uses Suricata as a network intrusion detection and prevention engine to inspect network traffic and identify application-related TLS fingerprints.

The lab was implemented on Ubuntu 22.04 using a wireless network interface (`wlo1`).

## 2. Architecture Components

| Component | Purpose |
|---|---|
| Ubuntu 22.04 | Lab operating system |
| AnyDesk | Application used as traffic-analysis subject |
| tcpdump | Network packet capture |
| tshark | TLS packet analysis |
| Suricata | IDS/IPS engine |
| JA3 | TLS client fingerprinting |
| iptables | Linux packet filtering |
| NFQUEUE | Passes selected packets to Suricata |

## 3. Detection Workflow

1. AnyDesk generates network traffic.
2. The network interface captures packets.
3. tcpdump stores traffic in PCAP format.
4. tshark inspects TLS ClientHello messages.
5. JA3 fingerprint information is extracted.
6. A custom Suricata rule checks the observed fingerprint.
7. Suricata generates an alert when the fingerprint matches.

## 4. Inline Inspection Experiment

The project also explored inline inspection using NFQUEUE.

The experimental traffic flow was:

```text
Outbound Network Traffic
          |
          v
   Ubuntu Interface
          |
          v
      iptables
          |
          v
       NFQUEUE
          |
          v
       Suricata
          |
     +----+----+
     |         |
   ACCEPT     DROP
     |         |
     v         v
   Network   Blocked
```

The NFQUEUE experiment demonstrated packet drops, but the available logs did not conclusively associate those drops with the custom AnyDesk signature.

## 5. Important Considerations

- JA3 is not a unique application identifier.
- TLS fingerprints may change with software updates.
- Offline PCAP detection and live IPS prevention are separate validation stages.
- Production deployment requires additional testing for false positives and application compatibility.
