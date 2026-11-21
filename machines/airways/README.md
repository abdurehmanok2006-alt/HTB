# HTB Airways — Hard

**Target:** `airways.htb`  
**Difficulty:** Hard (as provided; verify against your HTB profile)  
**Environment:** Linux foothold → Windows Active Directory  
**Status:** Retirement status not verified. Do not publish publicly until you confirm the machine is retired and publication is permitted under Hack The Box rules.

> This repository copy is a high-level, non-operational summary. It omits executable payloads, commands, credentials, and step-by-step exploitation instructions. Replace or verify machine metadata against your own HTB profile.

## Summary

The supplied account describes an initial foothold through a GeoNetwork formatter weakness, followed by pivoting from the Linux host into an internal Active Directory network. It then reports escalation through an SMB account-password change and an AD Certificate Services certificate-template misconfiguration, ending in domain-administrator access.

The exact exploitability and technical details have not been independently verified here.

## Attack path at a glance

1. **Reconnaissance:** Identify the GeoNetwork web service and its formatter functionality.
2. **Initial access:** Abuse unsafe processing of uploaded XSLT content to execute code in the service context.
3. **Pivot:** Use the compromised Linux host as a route to the internal Windows network.
4. **Credential access:** Obtain or change a low-privileged domain account's password through SMB account-management functionality.
5. **ADCS escalation:** Identify a certificate-template configuration that allows an enrolled certificate to represent a more privileged identity.
6. **Domain access:** Use certificate-based authentication to reach privileged domain access and retrieve the final flag.

## Findings and remediation

| Area | Risk | Recommended mitigation |
|---|---|---|
| GeoNetwork formatters | Untrusted stylesheet processing can enable code execution in the application context. | Upgrade GeoNetwork; restrict formatter creation to trusted administrators; disable or constrain processor extensions; isolate the processing service. |
| SMB account management | Remote password-change functionality can be abused when an attacker has suitable credentials or access. | Enforce strong credential practices, restrict unnecessary remote account-management access, and monitor anomalous SAMR activity. |
| ADCS templates | Overly permissive certificate enrollment and identity controls can allow impersonation. | Audit templates and enrollment permissions; prevent requester-controlled identity fields where not required; require approval or authorized signatures where appropriate; monitor certificate issuance. |
| Network segmentation | A foothold on one host may provide access to internal identity infrastructure. | Limit host-to-host and service-to-service connectivity; monitor and control tunneling paths. |

## Lessons demonstrated

- Web-application attack-surface review
- Secure handling of uploaded transformation files
- Network segmentation and pivot-risk assessment
- SMB account-management monitoring
- ADCS template auditing and certificate-based identity security

## Files not included

No scan output, verified exploit files, screenshots, license text, or independent scratch notes were supplied, so those files have been omitted. This summary intentionally does not include working payloads or an end-to-end exploitation script.
