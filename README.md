# Overleaf Trust Center - starter repo (DRAFT)

Starting point for Overleaf's FedRAMP Certification Data Sharing (CDS) build, Obtain date 2027-01-01. See the companion fedramp-vdr-ver-handoff-brief.md for full context.

## What this is

A lean, git-native pattern modeled on the patrick-tarly-co/trust-center reference repo (https://github.com/patrick-tarly-co/trust-center): versioned JSON files as the source of truth, a minimal static page to render them, and eventually a nightly pipeline that regenerates the JSON and commits it, so git history becomes the audit trail of Overleaf's posture over time.

This is NOT a finished product. Every file contains a comment or TODO field flagging what's real vs placeholder. Nothing here should be published publicly until reviewed.

## What's real vs placeholder

Pulled from the live FedRAMP marketplace listing (verified 2026-09-23, https://www.fedramp.gov/marketplace/products/FR2414718666): FedRAMP ID, legal entity name (Digital Science UK Limited), product website, certification type/path/class (Rev5, Agency, Class B), phase (Ongoing Certification), status (FedRAMP Certified as of 2025-01-29), authorization count (3).

Everything else is a TODO placeholder, including: UEI number, sales/security contact names, full service description, per-service security-category breakdown, SCG public link, documentation overview, next Ongoing Certification Report date, and the current 3PAO/assessor name.
