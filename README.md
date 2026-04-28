# ARIA Protocol

**Agent Registry for Identity & Authorization**

The open protocol for AI agent identity. DNS-anchored. Post-quantum native. Governed by a nonprofit. Working today.

[![Protocol Version](https://img.shields.io/badge/protocol-v1.1-00D4AA)](https://aria.bar/spec)
[![License: Apache 2.0](https://img.shields.io/badge/code-Apache%202.0-blue)](LICENSE-code)
[![License: CC BY 4.0](https://img.shields.io/badge/docs-CC%20BY%204.0-blue)](LICENSE-docs)
[![NIST Filing](https://img.shields.io/badge/NIST-2025--0035-orange)](https://aria.bar/nist)
[![npm](https://img.shields.io/npm/v/@aria-registry/verify)](https://www.npmjs.com/package/@aria-registry/verify)

---

## What is ARIA?

ARIA gives AI agents a verifiable, cryptographically signed identity — a passport for the agentic web.

Every agent gets a DID (`did:aria:*`), a trust level (L0–L3), a set of scopes defining what it can do, and a signed credential (AID) that any system can verify offline using post-quantum cryptography.

**The Agent Trust Protocol (ATP)** defines how receiving systems evaluate agent credentials against published DNS policies — like DMARC for AI agents.

## Quick links

| Resource | URL |
|----------|-----|
| **Live spec** | [aria.bar/spec](https://aria.bar/spec) |
| **API (public, no auth)** | [api.aria.bar](https://api.aria.bar) |
| **Registry (sign up)** | [registry.aria.bar](https://registry.aria.bar) |
| **SDK (npm)** | [@aria-registry/verify](https://www.npmjs.com/package/@aria-registry/verify) |
| **NIST filing** | NIST-2025-0035, March 9, 2026 |
| **NCCoE response** | AI-Identity@nist.gov, April 2, 2026 |

## Verify an agent in 3 lines

```bash
npm install @aria-registry/verify
```

```typescript
import { verifyAgent } from '@aria-registry/verify';

const result = await verifyAgent(credential);
console.log(result.valid, result.did, result.trustLevel);
```

Or via API:

```bash
curl https://api.aria.bar/v1/verify/did:aria:aria.bar:u-cmDoHhM3:ordering-agent
```

## Protocol overview

ARIA is a six-layer protocol:

| Layer | Standard | Purpose |
|-------|----------|---------|
| **P1 — Anchor** | W3C DID Core | Every agent gets a DID anchored to DNS |
| **P2 — Certify** | W3C VC Data Model 2.0 | Signed, portable, offline-verifiable credentials |
| **P3 — Present** | OAuth 2.0 + DPoP | How credentials are presented and bound |
| **P4 — Protect** | FIPS 204 + RFC 8032 | ML-DSA-65 + Ed25519 composite signatures |
| **P5 — Revoke** | StatusList 2021 | Real-time revocation via Trust Ledger |
| **P6 — Govern** | TrustLayer Foundation | Nonprofit stewardship, open source |

## Trust levels

| Level | Name | Verification | Credential validity | Status |
|-------|------|-------------|-------------------|--------|
| **L0** | Anchored | Cryptographic identity. Self-service. No DNS required. | 366 days | **Live** |
| **L1** | Identified | DNS-anchored. Email-verified principal. An identified person controls this agent. | 366 days | Coming soon |
| **L2** | Certified | Organization verified via DoH. vLEI-compatible. | 200 days | Coming soon |
| **L3** | Sovereign | Legal entity. Government registry. HSM. | 180 days | Coming soon |

## AID schema highlights (v1.1)

Each AID is a W3C Verifiable Credential signed with the composite cryptosuite
(`mldsa65-ed25519-2026`). Notable fields:

| Field | Purpose |
|---|---|
| `id` (top-level) | Unique credential-instance URL — `https://api.aria.bar/v1/credentials/{uuidv7}`. New on every issuance. Equivalent to a TLS certificate serial number. Per W3C VC 2.0 §4.4. |
| `credentialSubject.id` | The agent DID (`did:aria:…`). Stable across reissuances. |
| `credentialSubject.spec_version` | `"1.1"` for AIDs conforming to this schema. |
| `credentialSubject.previousCredentialId` | URL of the prior credential instance this one supersedes. Optional — omitted on first issuance. Enables explicit, signed chain-of-issuance traceability. |
| `credentialSubject.principal.verificationStatus` | Machine-readable provenance of `principal.legalName`. Enum: `self-declared` (L0, L1), `registry-confirmed` (L2 — vLEI-cross-checked), `legal-verified` (L3 — government documents + admin review). Verifiers MUST consult this before treating `legalName` as authoritative. |
| `credentialSubject.trustLevel` | `L0`–`L3`. |
| `credentialStatus` | W3C Bitstring StatusList 2021 entry — flipped to revoked atomically on every reissue. |
| `proof.proofValue` | Composite ML-DSA-65 + Ed25519 signature; both must verify. |

See [`schema/aid-v1.json`](schema/aid-v1.json) for the canonical schema and
[`examples/aid-example.json`](examples/aid-example.json) for a complete
sample.

## Agent Trust Protocol (ATP)

ATP is the three-phase handshake between an agent and a receiving system:

1. **Declare** — Agent presents AID + intent declaration
2. **Evaluate** — Receiver checks against DNS policy (`_aria-policy.<domain>`)
3. **Admit** — Pass or reject, returned as an ATP response code. The
   admit/reject decision is queued for the Agent Interaction Log (future
   — see spec §05). Credential lifecycle events are recorded separately
   in the Trust Ledger.

```
_aria-policy.bank.com TXT "v=ATP1; min=L2; enforce=strict; req=finance:*; intent=purpose,principal_ref"
```

Enforcement modes: `monitor` → `warn` → `strict` (graduated adoption, like DMARC).

## Cryptography

- **Primary:** ML-DSA-65 (FIPS 204) — post-quantum, lattice-based
- **Classical:** Ed25519 (RFC 8032) — transition signature
- **Mode:** Composite AND — both signatures must verify
- **Suite:** `mldsa65-ed25519-2026`
- **Sunset:** ECDSA/classical-only credentials expire December 31, 2029

## Standards alignment

| Standard | Mapping |
|----------|---------|
| NIST SP 800-63-4 (final, Aug 2025) | L0=IAL1, L1=IAL1, L2=IAL2, L3=IAL3 |
| FIPS 204 (ML-DSA) | Primary signature algorithm |
| RFC 8032 (Ed25519) | Classical composite signature |
| W3C DID Core | `did:aria` method |
| W3C VC Data Model 2.0 | AID document format |
| CA/B Forum SC-081v3 | L2 credential validity alignment |
| OWASP Agentic Security | Scope containment, delegation ceiling |
| EU AI Act | Risk category alignment, human oversight (Art. 13) |
| Colorado AI Act SB 205 | L2/L3 affirmative defense |

## Governance

ARIA is owned by **TrustLayer Foundation A.C.** (nonprofit) and operated by **TUNO Labs SAPI de CV** (for-profit, licensed).

Same model as Mozilla Foundation / Mozilla Corporation.

- **Spec + SDK:** Apache License 2.0
- **Documentation:** CC BY 4.0
- **Anti-capture clause:** No single entity can control the protocol

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). All contributions require DCO sign-off.

Until the Technical Steering Committee (TSC) is constituted (target Q3 2026), Aaron Grego and Ivan Moreno Mendoza act as TSC.

## License

Code and reference implementations: [Apache License 2.0](LICENSE-code)

Specification and documentation: [Creative Commons Attribution 4.0](LICENSE-docs)

---

**TrustLayer Foundation A.C.** · [aria.bar](https://aria.bar) · Open standards. Nonprofit governance. Trustworthy AI.
