# Project Limitations and Future Improvements

## 1. JA3 Fingerprint Limitations

JA3 fingerprints TLS ClientHello characteristics. They are not unique application identifiers.

Different applications may share the same fingerprint, and the fingerprint can change due to:
- Application version updates
- TLS library changes
- Operating system differences
- Changes in TLS configuration

Therefore, JA3 should be combined with other indicators before implementing production blocking policies.

## 2. Offline and Live Detection

Offline PCAP testing successfully generated an alert for the custom AnyDesk JA3 signature.

Live IPS testing demonstrated NFQUEUE initialization and packet drops. However, the available evidence did not conclusively associate those drops with the custom signature.

## 3. False Positive Considerations

Blocking traffic based only on a JA3 fingerprint could affect legitimate applications sharing that fingerprint.

Additional testing is required to evaluate false positives.

## 4. Future Improvements

- Test the signature against multiple AnyDesk versions.
- Compare fingerprints from different applications.
- Perform controlled live IPS testing with clear drop-event logging.
- Add additional detection indicators.
- Develop automated PCAP analysis scripts.
- Improve alert monitoring and reporting.

## 5. Conclusion

This project demonstrates practical network traffic analysis and custom IDS signature development. Further validation is necessary before using the experimental signature as a production prevention mechanism.
