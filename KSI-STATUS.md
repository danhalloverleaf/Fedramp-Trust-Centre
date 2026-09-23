# Overleaf - KSI Status

FedRAMP ID: FR2414718666
Generated: 2026-09-23 (placeholder - will be regenerated automatically once a real pipeline exists)

This table is generated from ksi-results.json. It is illustrative only - one row per theme, not the full catalog - and every status below is not-yet-evaluated because no automated evaluation pipeline has been built yet.

| KSI ID | Theme | Status |
|---|---|---|
| KSI-AFR-01 | Authorization by FedRAMP | not-yet-evaluated |
| KSI-IAM-01 | Identity and Access Management | not-yet-evaluated |
| KSI-CNA-01 | Cloud Native Architecture | not-yet-evaluated |
| KSI-SVC-01 | Service Configuration | not-yet-evaluated |
| KSI-MLA-01 | Monitoring, Logging, and Auditing | not-yet-evaluated |
| KSI-CMT-01 | Change Management | not-yet-evaluated |
| KSI-PIY-01 | Policy and Inventory | not-yet-evaluated |
| KSI-RPL-01 | Recovery Planning | not-yet-evaluated |
| KSI-INR-01 | Incident Response | not-yet-evaluated |
| KSI-CED-01 | Cybersecurity Education | not-yet-evaluated |
| KSI-TPR-01 | Third-Party Resources | not-yet-evaluated |

## How this should work once built

Per the Tarly trust center pattern (github.com/patrick-tarly-co/trust-center), this file and ksi-results.json should be regenerated nightly by a compliance pipeline running against production infrastructure, and committed unmodified - including KSIs not yet met, with remediation target dates. Git history then becomes the public record of how Overleaf's posture changes over time.
