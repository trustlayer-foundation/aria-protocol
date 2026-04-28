# Changelog

All notable changes to the ARIA Protocol specification.

> **Versioning note.** The public launch (April 1, 2026) shipped as **v1.0.0** — the canonical baseline reflected in the schema (`spec_version: "1.0"` at launch) and the README badge. Pre-launch entries below (`[1.3.0]` through `[1.6.0]`) document the iterative drafts that led to that launch and use the internal working numbering of that period; they predate the v1.0.0 public release. The current schema is **v1.1** (this changelog's `[1.1.0]` entry).

## [1.1.0] — April 27-28, 2026

### Added
- `principal.verificationStatus` (required) — machine-readable provenance of `principal.legalName`. Enum: `self-declared` (L0, L1), `registry-confirmed` (L2), `legal-verified` (L3). Closes the L1 brand-impersonation gap surfaced in the April 14 adversarial audit: a verifier can now reject AIDs whose org name was self-asserted without consulting trustLevel separately.
- `credentialSubject.previousCredentialId` (optional) — URL of the prior AID instance superseded by this one. Enables explicit, signed chain-of-issuance: a verifier holding a current AID can walk backwards through the issuance history. Omitted on first issuance.

### Changed
- `spec_version` const: `1.0` → `1.1`. AIDs claiming conformance to v1.1 MUST include `principal.verificationStatus`. Existing v1.0 AIDs remain valid against the v1.0 schema; v1.1 verifiers SHOULD treat a missing `verificationStatus` as `self-declared`.
- **Top-level `id` is now a unique credential-instance URL per W3C VC 2.0 §4.4** (e.g. `https://api.aria.bar/v1/credentials/{uuidv7}`). Previously the schema reused the agent DID for both the credential `id` and `credentialSubject.id`, which conflated the credential instance with its subject. The agent DID continues to live in `credentialSubject.id` (stable across reissuances). The new top-level `id` is the equivalent of a TLS certificate serial number — unique per issuance, dereferenceable, signed inside the proof.

### Migration notes
- v1.0 AIDs with `id: "did:aria:..."` will not match the v1.1 schema regex for top-level `id`. Reissuance produces a v1.1-conformant AID. v1.0 verifiers continue to accept their own AIDs.
- Implementations SHOULD use UUIDv7 (RFC 9562) for the credential identifier so issuances are chronologically ordered.

## [1.6.0] — April 1, 2026

### Fixed
- B-01: L0 trust level no longer requires DNS (crypto keypair only)
- B-02: Trust Ledger scoped to credential lifecycle events only
- B-03: §09 Trust Ledger definition — removed ATP event overclaims
- B-04: ATP Phase 3 references Agent Interaction Log (future), not Trust Ledger
- B-05: ATP warn mode — references Agent Interaction Log
- B-06: Added qualify= and ttl= to ATP DNS tag table
- B-07: DID CREATE — L0 does not create DNS record (L1+ only)
- B-08: Auth Factor 1 (DNS domain control) applies to L1+, not L0
- B-09: "CTL records" → "Trust Ledger records" in §18
- B-10: §20 Audit — scoped to lifecycle events, ATP rows removed
- B-11: Scope format harmonized to "namespace:resource:action"
- B-12: ATP §XX.5 renamed to "Agent Interaction Log [PLANNED]"
- B-13: Removed pricing from spec (pricing does not belong in protocol spec)
- B-14: Glossary — 6 definitions corrected for Trust Ledger scope

### Added
- N-01: Three-role identity model (Registrant, Principal, Account Admin)
- N-02: Colorado AI Act SB 205 mapping, AAL column, EU AI Act 5-obligation mapping
- N-03: HITL tiers connected to EU AI Act Article 13 human oversight
- N-04: Trust Seals — three categories (Sector, Compliance, Capability)
- N-05: Insurance framework note (qualify=insured)

### Verified
- V-01: SDK status — TypeScript live, Python/Go [PLANNED Q2 2026]
- V-02: All resolver.aria.bar references changed to api.aria.bar
- V-03: @context URL updated to aria.bar/ns/v1.0, spec_version = 1.6

## [1.5.0] — March 23, 2026

### Added
- Agent Trust Protocol (ATP) — three-phase handshake (Declare, Evaluate, Admit)
- ATP DNS record format (_aria-policy.<domain>)
- ATP response codes (200, 401, 403, 406, 429, 451, 460, 462)
- Intent Declaration schema (purpose, principal_ref, action_requested, etc.)
- Trust Ledger terminology (replaces Credential Transparency Log)
- Trust Record terminology (single entry in Trust Ledger)
- Enforcement modes: monitor, warn, strict
- Credential states: active, suspended, revoked, expired, tombstoned, superseded

### Changed
- §11 Intent Architecture absorbed into ATP §XX.1
- §18 Counterparty Policy Declaration absorbed into ATP §XX.2

## [1.4.0] — March 20, 2026

### Changed
- Cryptography: adopted composite AND model (ML-DSA-65 + Ed25519)
- Trust levels: adopted operational verification ladder (email → DNS → legal)
- Scopes: adopted 8 immutable actions model
- DNS re-verification aligned with CA/B Forum SC-081v3

## [1.3.0] — March 9, 2026

- Initial specification filed with NIST (NIST-2025-0035)
