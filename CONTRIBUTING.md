# Contributing to ARIA Protocol

Thank you for your interest in contributing to the ARIA Protocol specification.

## How to contribute

### Spec questions
Open an issue with the `spec-question` label. We'll respond within 7 business days.

### Bug reports
If you find an error in the spec (incorrect reference, inconsistency, typo), open an issue with the `bug` label.

### Public comments
Proposed changes to the spec go through a public comment process. Open an issue with the `public-comment` label. Include:

1. The section number (e.g., §05 ATP)
2. The current text
3. Your proposed change
4. Rationale

### Code contributions
For SDK contributions, see [@aria-registry/verify](https://github.com/trustlayer-foundation/aria-verify).

## DCO Sign-Off

All contributions require a Developer Certificate of Origin (DCO) sign-off. Add the following to your commit message:

```
Signed-off-by: Your Name <your.email@example.com>
```

Or use `git commit -s` to add it automatically.

## Spec change process

| Change type | Process |
|-------------|---------|
| Typo / wording only | Direct fix. Patch version (v1.1.x). No public comment. |
| New optional field, backward-compatible | GitHub Issue with `public-comment` label. 90-day comment window. TSC decides. Minor version (v1.1). |
| Trust level or ATP change | Same as above. Existing credentials grandfathered 36 months. |
| Breaking change | Major version (v2.0). 90-day minimum comment period. Supermajority TSC vote. 18-month deprecation notice. |
| Emergency security fix | Immediate fix. 30-day retroactive public comment. TSC ratifies. |

## Governance

Until the Technical Steering Committee (TSC) is constituted (target Q3 2026), Aaron Grego and Ivan Moreno Mendoza act as interim TSC.

## Code of conduct

Be respectful. Be constructive. Focus on the technical merits.
