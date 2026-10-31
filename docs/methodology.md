# Methodology: AnyDesk Traffic Analysis

## 1. Objective

The objective was to identify AnyDesk-related encrypted network traffic and develop a custom Suricata signature using TLS fingerprinting.

## 2. Traffic Capture

Network traffic was captured on the Ubuntu wireless interface using tcpdump.

```bash
sudo tcpdump -i wlo1 -nn -s0 -w anydesk-capture.pcap
```

## 3. TLS Analysis

The captured PCAP was analyzed using tshark to identify TLS ClientHello messages.

```bash
tshark -r anydesk-capture.pcap -Y "tls.handshake.type == 1"
```

The analysis identified an ALPN value:

`anydesk/6.3.0/linux`

## 4. JA3 Fingerprint Extraction

The TLS ClientHello was inspected to extract its JA3 fingerprint.

Observed fingerprint:

`29b5a018fa5992fe23560c16af0dc9fc`

The fingerprint was observed again in a fresh capture associated with an AnyDesk process.

## 5. Suricata Rule Development

A custom Suricata rule was created using the JA3 hash keyword.

```text
alert tls any any -> any any (msg:"LOCAL AnyDesk JA3 fingerprint detected"; ja3.hash; content:"29b5a018fa5992fe23560c16af0dc9fc"; sid:1000001; rev:2;)
```

## 6. Rule Validation

The custom rule was tested using Suricata's configuration test mode.

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -S /etc/suricata/rules/local.rules
```

The configuration loaded successfully.

## 7. Offline Detection

The custom signature was tested against the captured PCAP using Suricata offline analysis.

A matching alert was generated, confirming that the fingerprint was detected.

## 8. Inline Inspection Experiment

NFQUEUE was configured to route outbound TCP port 443 traffic through Suricata for inline inspection.

IPS packet drops were observed during testing. However, the available evidence did not conclusively link those drops to the custom AnyDesk signature.

## 9. Conclusion

The lab successfully demonstrated TLS traffic analysis, JA3 fingerprint extraction, custom Suricata signature development, and offline application fingerprint detection.
