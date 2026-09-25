# Cretes RFCs

Requests for Comments provide the durable record for significant Cretes changes. This repository establishes the process only; there are no accepted language-design RFCs yet.

## When an RFC is required

Use an RFC for language semantics, type or memory models, concurrency, public interfaces, compatibility policy, major runtime/compiler architecture and governance changes. Small bug fixes and editorial corrections may use ordinary issues and pull requests.

## Process

1. Open an issue describing the problem and check for related proposals.
2. Copy [TEMPLATE.md](TEMPLATE.md) into `proposals/0000-short-title.md` on a branch. Use 0000 until a maintainer assigns the next unused number; numbers are never reused.
3. Submit a pull request. Complete motivation, design, alternatives, compatibility, safety and validation sections. Clearly mark sketches as unimplemented.
4. Discuss tradeoffs publicly. Update the proposal as review progresses.
5. The lead announces a final-comment period of at least seven calendar days, states the intended outcome and addresses outstanding objections.
6. Record Accepted, Rejected or Deferred with date, rationale and links. Keep rejected/deferred history accessible; merge it as a decision record when useful.
7. For an accepted proposal, link separate specification, implementation, test and release tracking. Acceptance does not mean implementation or release.

## Lifecycle

Draft → Review → Final comment → Accepted / Rejected / Deferred.

Accepted proposals may later become Superseded or Withdrawn with a linked decision. Track implementation separately as Not started, In progress, Implemented or Released. Do not silently edit a decided RFC to change meaning; use a follow-up RFC.

Current lead: @krishanth7. Follow the single-maintainer review policy in governance. Security vulnerability details belong in private reporting, not public RFCs.

## Project policies

- [Contributing](https://github.com/Cretes-lang/.github/blob/main/CONTRIBUTING.md)
- [Governance](https://github.com/Cretes-lang/.github/blob/main/GOVERNANCE.md)
- [Code of Conduct](https://github.com/Cretes-lang/.github/blob/main/CODE_OF_CONDUCT.md)
- [Security reporting](https://github.com/Cretes-lang/.github/blob/main/SECURITY.md)
- [Engineering standards](https://github.com/Cretes-lang/.github/blob/main/ENGINEERING.md)
- [Versioning](https://github.com/Cretes-lang/.github/blob/main/VERSIONING.md)

Initial maintainer: @krishanth7. License: [Apache-2.0](LICENSE).
