<p align="center">
  <img width="295.3125" height="160.9375" alt="trustpoint_logo" src="https://github.com/user-attachments/assets/e9cb87ee-a8e6-4f07-b6a9-4a4ac5f524ea" />
</p>

---

## What is trustpoint?

trustpoint is an open-source device identity manager for modern zero trust networks. It uses mutual TLS (mTLS) as its backbone to assign each device a unique, cryptographically verifiable identity, which can be dynamically revoked if its posture changes or it becomes compromised.

## Security Enhancements

The table below outlines the security enhancements offered by trustpoint, compared to a standard reverse proxy + forward authentication setup (traditional network boundary).

| Feature                | Traditional Network Boundary                                                                                                                     | trustpoint Network Boundary                                                                                               |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| Access model           | Reactive security - allows access until trust is violated, then blocks the attacker                                                            | Proactive - denies all requests by default until the device verifies its identity                                       |
| Authentication layer   | User based only - requires the user to verify their identity before granting access                                                           | User and device bound - session identity is tied to both the device’s certificate and the user account                 |
| Trust basis            | Implicit - relies on IP reputation, passwords, and sometimes MFA                                                                               | Explicit - cryptographic identity bound to both device and user account                                                |
| Blocking               | Delayed and imprecise - sessions wait for timeouts, or IPs must propagate on blocklists, multiple users sharing an IP (CGNAT) may be affected | Instant and precise - SIEM can immediately revoke the offending device and user identity via OCSP based on trust score |
| Visibility             | Limited - if the device isn’t signed in, logs are tied only to IP addresses                                                                   | Increased - logs are bound to the device identity, including owner account, model, and properties                      |
| Resistance to spoofing | Weak - relies on IPs, which can be rotated or masked via VPN                                                                               | Strong - cryptographic identity cannot be spoofed or phished, and is harder to clone                                   |
| SIEM integration       | Limited - only basic alert correlation                                                                                                        | Native - provides device properties to the SIEM and enables automated revocation                                       |
| Zero-trust alignment   | Partial - grants initial trust to clients before checks, all devices receive a baseline level of trust                                        | Full - no trust is given until the device and current posture is verified, unknown devices are denied by default       |

## Features

See [here](https://m-akrami.notion.site/2924d6e1dc0880f58bd2db7bb73bf8fe?v=2924d6e1dc08805f8e83000c3061069a&source=copy_link) for planned releases and feature updates.
