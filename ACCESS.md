# Access Log - Overleaf Trust Center

FedRAMP ID: FR2414718666

Per CDS-TRC, this trust center must maintain an inventory/history of which federal agency users or systems have accessed Overleaf's certification data, available to FedRAMP on request, with access logging retained 6+ months.

Status: not yet implemented. ACCESS.json shows the intended shape of a log entry. Before this is real, the team needs to decide:

1. Access model - CDS-TRC prefers just-in-time provisioning (CDS-TRC-USH: uninterrupted/no manual approval gate per request) and agency-managed users (CDS-TRC-SSM: agencies manage their own users) rather than Overleaf provisioning each requester by hand.
2. 2. What counts as an access - every file fetch via the public repo/API, or only requests for the full certification package (SSP, POA&M, pentest summary) beyond the always-public CDS-CSO-PUB data?
   3. 3. Where this lives operationally - if the trust center ships as static JSON in a public repo (this starter's pattern), anonymous public reads of fedramp.json/ksi-results.json can't be individually logged the way an authenticated API can. The full certification package access (gated) is where per-agency logging is most feasible and most clearly required.
      4. 4. Retention and access controls on the log itself - the log contains information about who is requesting Overleaf's compliance data, which is itself sensitive; it shouldn't be fully public.
        
         5. This is flagged as an open design question for whoever owns the Trust Center build - it's more of an architecture decision than a coding task.
