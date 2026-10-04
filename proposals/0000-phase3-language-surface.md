# RFC 0000: Phase 3 language surface and v0.1 candidate

- Author: krishanth7 (AI-assisted drafting)
- Created: 2026-10-03
- Status: PROPOSED / UNACCEPTED
- Decision date and rationale: Pending
- Discussion: the pull request introducing this proposal
- Tracking: https://github.com/Cretes-lang/spec/issues/8
- Implementation: Not started; isolated grammar recognizer only
- Supersedes: None

## Summary

Propose the first Cretes source-language candidate, covering sections 3.1–3.40. The specification package is `spec/docs/language/` on branch `docs/phase3-language-surface` until its publication PR merges. Publication of drafts is not acceptance, a freeze, or a release. Phase 2 architecture acceptance remains pending in https://github.com/Cretes-lang/rfcs/pull/1; this proposal cannot silently override that outcome.

## Motivation and requirements

Phase 1 establishes 257 requirements and an 86-entry v0.1 target. Phase 2 proposes architecture but leaves source spellings and grammar to Phase 3. A single reviewable surface prevents later compiler work from inventing incompatible rules. `TRACEABILITY.md` maps every requirement, retaining original priority and target, to its architecture response and candidate surface or separate implementation/library/policy obligation. Traceability does not claim fulfillment.

## Proposed design

Read `README.md` for the 40-section index and `SPEC.md` for NORMATIVE CANDIDATE rules. The proposed language uses strict UTF-8 source with original byte spans, ASCII identifiers, braces and mandatory semicolons, nested comments, strict numeric/text/character/byte literals, and 26 keywords. Fixed-width numeric types, text/bytes, sequences/maps/sets, tuples, records, tagged sums, Option and Result form the initial type surface.

Bindings use let, var and restricted const. Functions use typed positional parameters, explicit return types and return statements. Statements include if/else, while, loop, restricted sequence/bytes for, and exhaustive match. Records use `new Type { ... }`; enum constructors are qualified and parenthesized. Imports use package-qualified paths, aliases and explicit pub visibility without import execution.

Move semantics, explicit shared/exclusive borrows and single-source returned-reference provenance expose the architecture's resource model. Result propagation uses postfix `?` with the exact error type; explicit discard requires a nonempty reason. Arithmetic is checked; no implicit null or unchecked source escape is introduced. Scope cleanup applies to normal exits, with faults distinguished from recoverable failures.

`cretes.ebnf` contains the token grammar, `lexical.ebnf` the character-level definitions. Semantic constraints remain in prose. The 14 examples and conceptual standard-library contracts are explanatory, not delivered APIs. Methods, user generics/interfaces, function values, async/concurrency, unsafe/FFI and attributes remain deferred or post-v0.1. `V0.1-SYNTAX.md` records the exact boundaries and acceptance checklist.

## Alternatives and tradeoffs

`DECISIONS.md` compares alternatives for 22 design families. Braces/semicolons cost punctuation but avoid newline insertion rules. ASCII names restrict international naming while simplifying initial source-security behavior. Square-bracket type arguments avoid angle-token splitting. Explicit `new` avoids record/control-block ambiguity. Explicit returns avoid a second tail-value model. `from parameter` restricts returned loans to one provenance source. Deferring broader features reduces initial grammar and checker obligations while preserving future RFC review.

## Compatibility and migration

There is no released source syntax to migrate. This candidate creates no stable ABI, implementation compatibility or 1.0 guarantee. Original requirements and architecture statuses remain intact. Later incompatible draft changes require rationale and coordinated grammar/example updates. Acceptance must identify exact specification and RFC revisions.

## Security, privacy and resource safety

The proposal rejects malformed UTF-8, source BOMs, bare CR and specified raw controls/bidi/invisibles; no silent source normalization is allowed. Identifiers and literal grammar are explicit. Bytes and text are distinct; integer operations and indexing require checked behavior. Ownership, borrowing, Result obligations and exhaustive patterns require a future semantic checker; parser success is not a safety proof. FFI/unsafe source constructs are absent. The conceptual examples contain no live targets, credentials or exploit execution. `REVIEW.md` records security, parser, tooling and teaching limitations.

## Performance and portability

Fixed precedence and explicit contracts aim to simplify parsing and cross-module analysis. No speed, memory or readability benchmark is claimed. UTF-8 byte spans and target-independent numeric spellings support portable tooling; usize remains explicitly target-sized. Four-domain examples test expressibility only, not network, cryptographic, GPU or runtime performance.

## Specification, implementation and validation

The package contains 40 sections, 14 examples, 45 reachable named syntax rules, 101 fixtures and 257 traceability mappings. Python 3.10+ standard-library `validate.py` is an isolated lexer/EBNF recognizer with no production AST, type checker, execution, backend or runtime. It recognizes 14 examples and 32 valid fixtures, rejects 31 syntax and 22 lexical negatives, and intentionally parses 16 semantic-negative fixtures reserved for future checker tests. UTF-8 rejection, original-byte CRLF spans and the 26-keyword inventory are checked. Local preparation checked 866 relative Markdown paths/anchors against the baseline.

The lexical grammar is manually reviewed; only token grammar is consumed by the recognizer. This is not a global ambiguity proof, soundness proof, production fuzzing result or conformance certification. Remote publication verification and exact revision evidence belong in the publication PR and tracker. No Phase 4 work is included.

## Documentation and teaching

`EXAMPLES.md` indexes canonical syntax and states conceptual API contracts. `DOMAINS.md` evaluates automation, networking, AI/ML preprocessing and defensive bytes. `COMPLETION.md` and `VALIDATION.md` distinguish completed candidate work from remaining acceptance and implementation. All normative candidates and non-normative explanations remain explicitly labeled.

## Unresolved questions

Architecture acceptance must resolve first. Public review should test the restricted returned-borrow model, ASCII identifier policy, mandatory punctuation, constructor syntax, diagnostics and scope boundaries. Manifest format remains a separate architecture gate. Standard-library contracts and semantic implementation are future work; no missing implementation is disguised as a syntax decision.

## Decision record

Proposed on 2026-10-03. No acceptance, rejection or external approval is recorded. After public discussion, announce the governance-required final-comment period with intended outcome, address objections and record a dated decision tied to exact revisions. Merge of this proposal or its specification draft must not be interpreted as acceptance. Phase 4 remains unstarted.
