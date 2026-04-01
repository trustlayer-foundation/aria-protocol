# ARIA Protocol

**Agent Registry for Identity & Authorization**

The open protocol for AI agent identity. Post-quantum cryptography. Offline verification. Zero trust assumptions.

[![Protocol Version](https://img.shields.io/badge/protocol-v1.0-00D4AA)](https://aria.bar/spec)
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
| **L0** | Anchored | Account created + email verified. Self-service. | 366 days | **Live** |
| **L1** | Identified | Domain ownership proven via DNS TXT. | 366 days | Coming soon |
| **L2** | Certified | Legal entity verified. Government registry cross-reference. | 200 days | Coming soon |
| **L3** | Sovereign | Government-attested identity. | 180 days | Coming soon |

## Agent Trust Protocol (ATP)

ATP is the three-phase handshake between an agent and a receiving system:

1. **Declare** — Agent presents AID + intent declaration
2. **Evaluate** — Receiver checks against DNS policy (`_aria-policy.<domain>`)
3. **Admit** — Pass or reject, logged to Trust Ledger

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
| NIST SP 800-63-4 (final, Aug 2025) | L0=IAL1, L1=IAL1+, L2=IAL2, L3=IAL3 |
| FIPS 204 (ML-DSA) | Primary signature algorithm |
| RFC 8032 (Ed25519) | Classical composite signature |
| W3C DID Core | `did:aria` method |
| W3C VC Data Model 2.0 | AID document format |
| CA/B Forum SC-081v3 | L2 credential validity alignment |
| OWASP Agentic Security | Scope containment, delegation ceiling |
| EU AI Act | Risk category alignment, human oversight (Art. 13) |
| Colorado AI Act SB 205 | L2/L3 affirmative defense |

## Governance

ARIA is owned by **TrustLayer Foundation A.C.** (nonprofit) and operated by **TunoLabs** (for-profit, licensed).

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
