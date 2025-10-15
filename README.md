# TrustPoint

Device identity manager for zero-trust networks. Issue mTLS certificates, bind them to device fingerprints, and enforce dynamic access control.

### Why TrustPoint?

In traditional networks, exposed services often rely on reactive defences such as bans after repeated failed logins, rate limiting, blocking honeypot hits, GeoIP restrictions, and ASN filtering.

While these measures can reduce some risk, they still allow attackers to reach your services in the first place. This reactive “whack-a-mole” approach leaves a window for reconnaissance, credential abuse, and device spoofing, and it does not align with modern zero-trust principles, by giving every device some level of trust to access the service in the first place, then revoking that trust once it is abused.

For my lab, I wanted to design a solution that identifies devices before they even establish a connection, mitigating malicious outsider attacks before they even hit the service. TrustPoint allows administrators to enforce a deny-by-default approach in conjunction with their reverse proxy, requiring devices to present a unique mTLS certificate which becomes their hardware fingerprint, including serial number, model, OS version, and other attributes saved at enrolment, before gaining access. By combining device-bound certificates with active monitoring, TrustPoint allows a SIEM to immediately burn a compromised device in real time, terminating the session and preventing further access, by comparing current device posture to the state of the device when it was enrolled.

This approach mitigates multiple attack vectors, including stolen or cloned devices, credential misuse, OS/browser downgrade attacks, device sharing, and reconnaissance attempts. All this ensures that only verified devices can connect, providing zero-trust protection across the network perimeter while giving administrators full visibility and control, by tying website access events and attacks to a specific device, and by extension a user, rather than a random IP address.

### Example Scenario

Charlotte owns an iPhone, and Bob owns an iPad. Both devices have been enrolled by an IT admin (TrustPoint device fingerprint certificate generated, MDM configured, managed VPN set up, WebAuthn credentials enrolled). With this setup, Charlotte or Bob can access their company website (sales.example.com). The firewall, reverse proxy, and WAF enforce multiple checks:

- Is the request coming from an expected network ASN?
- Does the `User-Agent` match a value possible for the device type?
- Does the request comply with WAF rules (e.g., rate limits, allowed endpoints)?
- Did the client pass WebAuthn authentication?

If an attacker steals Charlotte’s WebAuthn credentials, they are useless without the corresponding device fingerprint certificate. The account and enrolled devices can be blocked automatically. If the attacker exfiltrates the certificate without the WebAuthn credentials, the SIEM will detect a policy violation, therefore device tampering, dropping the trust score and blocking both the account and device.

Even if an attacker steals both the certificate and WebAuthn credentials, they must be on an expected network to pass ASN checks. 

Any anomalous activity, such as triggering honeypots or failing posture checks, can be traced back to the stolen certificate and account, allowing instant suspension. This ensures that attacks require matching device, credentials, and network posture, limiting feasible attack vectors.

If Bob was behind the same CGNAT as Charlotte (same household or geographic location), the actions taken can be precise and targeted without affecting other users, minimising disruption, compared to a traditional IP ban.

### TrustPoint’s role in the stack

TrustPoint takes multiple sources of information (users via LDAP, device list database) to bind a certificate to a user account and device properties. Once the certificate has been minted, the private key is not stored server-side, preventing impersonation. On session start, the reverse proxy should query TrustPoint via the OCSP protocol, and if
