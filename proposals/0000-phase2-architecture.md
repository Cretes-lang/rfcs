# RFC 0000: Cretes Phase 2 architecture baseline

- Author: @krishanth7, with AI-assisted drafting and documented self-review
- Created: 2026-09-27
- Status: Review — architecture decisions PROPOSED
- Decision date and rationale: No acceptance decision recorded
- Discussion / pull request: This proposal's pull request; [Phase 2 tracker](https://github.com/Cretes-lang/spec/issues/4)
- Implementation status: Not started
- Supersedes / superseded by: None

## Summary

Propose one coherent technical architecture covering Phase 2 sections 2.1–2.30. The detailed decision records, alternatives, requirements and consequences are in the [architecture proposal](https://github.com/Cretes-lang/spec/tree/docs/phase2-architecture/docs/architecture). This RFC incorporates that review revision; acceptance must identify the exact reviewed spec commit rather than silently accepting future branch changes.

The proposed core is native AOT compilation; a Rust-hosted modular compiler; static typing with local inference; move ownership and scoped borrowing; explicit option/result values; profile-independent checked integer behavior; typed HIR and ownership-aware MIR; an initial LLVM backend; and a small synchronous core runtime. Later structured async, C-ABI interop and domain packages extend the same resource/error model. None is implemented by this RFC.

## Motivation and requirements

Phase 1 recorded 257 requirements (173 MUST, 74 SHOULD, 10 MAY), 19 use cases, 15 principles and 28 open architecture questions. A compiler team needs an internally consistent response to these outcomes before grammar and implementation proceed. [TRACEABILITY.md](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/TRACEABILITY.md) maps every requirement, preserving its priority, target and future verification obligation. A mapping is a design response, not evidence of implementation compliance.

In particular, SAFE-023 requires an accepted memory-management RFC evaluating safety, deterministic resources, latency, FFI, secrets, concurrency and learning cost. It is not satisfied until this proposal, or a replacement memory proposal, is accepted through the existing process. v0.1 remains 86 requirements; later capabilities do not enter that scope merely because their architecture is discussed.

## Proposed design

The [30-record register](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/README.md) is the section-by-section design. Major records include:

| Decision | Proposal | Principal reason |
| --- | --- | --- |
| ARCH-EXEC-001 | Native AOT; run builds/caches then executes | Native deployment and one semantic engine |
| ARCH-COMPILER-001 | Modular compiler, initially Rust-hosted | Reusable tooling and reduced host memory-safety burden |
| ARCH-TYPE-001 | Static typing, local inference, checked numerics | Early errors and concrete layouts without global inference |
| ARCH-MEM-001 | Move ownership and scoped loans | Lifetime/alias control, resource and buffer integration |
| ARCH-RESOURCE-001 | Guards plus explicit fallible finish | Predictable cleanup on normal/error paths |
| ARCH-ERROR-001 | Results for recovery; abort-only v0.1 faults | Explicit propagation and a contained failure boundary |
| ARCH-NULL-001 | Non-null references plus option | Typed absence without implicit unchecked dereference |
| ARCH-IR-001 | Typed HIR and ownership-aware MIR | Preserve checks, cleanup and source origins |
| ARCH-BACKEND-001 | LLVM adapter initially | Native optimization/debug/ABI needs with isolated coupling |
| ARCH-RUNTIME-001 | Small linked core; optional later scheduler | Avoid mandatory scheduler/VM startup for scripts |
| ARCH-CONC-001 | Structured groups and checked transfer/share | Bound task lifetimes and prevent unsafe sharing |
| ARCH-ASYNC-001 | Explicit async, state machines, terminal-completion lifetimes | Make suspension/cancellation costs and ownership visible |
| ARCH-FFI-001 | C ABI with explicit unsafe contracts | Native integrations without exposing private layouts |
| ARCH-PLATFORM-001 | Linux x86-64 first candidate; tier gates | One validated initial target, explicit OS adapters |
| ARCH-BUILD-001 | Declarative local v0.1; future locked dependency graph | Reproducibility and no implicit installation execution |
| ARCH-SECURITY-001 | Explicit trust boundaries and provider review | Precise guarantee scope and updateable security components |
| ARCH-COMPAT-001 | Maturity/version metadata; no premature editions | Honest pre-1.0 evolution and artifact compatibility |

Other records cover the stage pipeline, safety, optimization, tools, diagnostics, debugging, all four domains, v0.1 and the architecture review. Every record remains PROPOSED. [Question dispositions](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/QUESTIONS.md) keep the original 28 questions visible until acceptance; some detail is deliberately deferred.

No surface syntax is selected. The parser strategy is conceptual; keywords, delimiters, operator precedence and declaration forms belong to Phase 3. No production source code, backend dependency, runtime, registry or release is created.

## Alternatives and tradeoffs

Execution compares AOT, VM, interpreter, JIT, transpilation, hybrid and WebAssembly. Types compare static, dynamic, gradual and global versus local inference. The [memory record](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/MEM.md) compares tracing GC, ownership, ARC, regions, manual allocation and hybrids against the SAFE-023 criteria. The [backend record](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/BACKEND.md) compares LLVM, Cranelift, GCC infrastructure, C output, custom codegen and hybrid backends. Error/concurrency/async alternatives are documented in their records.

Ownership reduces reliance on reclamation timing but imposes a real learning/checker cost and does not bound destruction latency. LLVM reduces code-generator construction work but adds dependency/build cost and dangerous lowering preconditions. Explicit async introduces function coloring; synchronous automation helpers reduce, but do not erase, this cost. Abort-only faults simplify v0.1 but do not promise destructor execution or secret erasure on termination. These tradeoffs must be accepted consciously rather than marketed away.

Doing nothing leaves the architecture unresolved. Implementing a throwaway compiler first risks hardening accidental semantics and violates the requested phase boundary. Adding multiple backends or a mandatory collector before evidence increases the verification surface; the proposal starts with one coherent implementation path.

## Compatibility and migration

No released language/compiler exists. Existing Phase 1 requirements, priorities and milestone targets remain unchanged. Private Cretes ABI remains unstable; future supported C-facing interfaces are versioned explicitly. Follow existing VERSIONING.md for release policy. Accepted architecture does not imply stable syntax, implemented features or a release.

## Security, privacy and resource safety

The proposed safety contract depends on a correct compiler/backend, trusted runtime/library wrappers, OS and native providers. It is not an assertion of proven safety. Resource guards clean up on result-error propagation; v0.1 fatal termination does not promise cleanup. Async cancellation retains buffers until terminal completion. Foreign declarations are unsafe assertions requiring wrappers, not automatically safe APIs.

Secret storage prevents ordinary implicit copies/formatting and uses non-elidable zeroization on normal release. Registers, OS state, foreign copies and abort paths remain limitations. Constant-time guarantees are scoped to reviewed primitives; arbitrary Cretes source receives no guarantee. Crypto/TLS provider selection requires separate expert review. Native programs are not sandboxes. Build/resolve/check cannot implicitly execute dependency code.

## Performance and portability

No Cretes benchmarks exist and no measurements are claimed. Future validation measures cold/warm compilation, startup, peak memory, destruction tail latency, task wakeups, I/O buffering, numerical throughput, FFI overhead and artifact size using recorded hardware/toolchain/profile settings. Qualitative comparisons and [primary source notes](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/SOURCES.md) explain hypotheses only.

Linux x86-64 is the first proposed target; Windows x86-64 and macOS ARM64 remain candidates requiring their own conformance and installation evidence. OS versions/SDKs and exact backend versions are release-readiness choices, not current support promises. Paths, processes, native handles, object/debug formats and I/O drivers have explicit adapters.

## Specification, implementation and validation

The proposed [v0.1 blueprint](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/V0.1-ARCHITECTURE.md) preserves all 86 original v0.1 requirements. It sequences frontend/diagnostics, semantic/ownership analysis, MIR/runtime/backend, libraries/CLI and conformance after architecture and syntax approval. It does not open implementation tasks prematurely.

Documentation validation checks 30 unique decision IDs and required record fields, all 257 requirement mappings including 173 MUSTs, the 86-target v0.1 set, 28 question dispositions and internal Markdown paths/anchors. [REVIEW.md](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/REVIEW.md) records language, compiler, security, performance and developer-experience self-review. No independent approval or executable test result is implied.

## Documentation and teaching

Provide reference explanations of moves, loans, result propagation, optional values and explicit expensive operations. Before shipping, evaluate small file-processing and CLI tasks with new users. Ownership diagnostics must show causes and lifetime endpoints, not merely demand annotations or cloning. Public documentation must distinguish proposed, accepted, implemented and released work.

## Unresolved questions

[RISKS.md](https://github.com/Cretes-lang/spec/blob/docs/phase2-architecture/docs/architecture/RISKS.md) assigns current ownership to the project lead until delegation is recorded. Deferred items include exact manifest/lock/resolver/registry formats, crypto/TLS provider selection and trust roots, generic coherence details, concurrency atomic/fairness specification, target minimum versions and accelerator contracts. These are explicit gates before their respective implementation/delivery, not silently resolved questions.

Formal loan/move rules, native lowering correctness and novice usability require further evidence. If review finds a fundamental mismatch with Phase 1, revise this RFC; do not weaken a MUST requirement implicitly. No research spike or production implementation has run.

## Decision record

Public review is open. No final-comment interval or acceptance outcome is backdated. The lead must announce an intended outcome and a final-comment period of at least seven calendar days, address objections and then record Accepted, Rejected or Deferred with date and rationale. At acceptance, assign the next unused RFC number, pin the reviewed specification commit, update architecture statuses and question links, and link the subsequent specification/implementation/conformance/release tracking separately.

Merging a clearly labeled proposed documentation PR is publication only. This RFC remains open until its decision gate is met. The Phase 2 tracker must not close merely because documents exist.
