---
name: universal-coding-standards
description: >-
  Applies default engineering conventions across all projects when writing,
  changing, fixing, refactoring, reviewing, or designing code, including
  dependency choices. Complements Ponytail and Caveman with architecture,
  correctness, C/C++ conventions, build checks, and commit discipline.
  Does not apply to unrelated prose or general knowledge requests.
---

# Universal Coding Standards

## Composition and precedence

Read alongside [Ponytail](../ponytail/SKILL.md) and
[Caveman](../caveman/SKILL.md), unless already loaded. Ponytail owns flow
analysis, root-cause fixes, reuse, minimal implementation, necessary safeguards,
and the runnable-check baseline. Caveman owns concise communication. This skill
adds the engineering conventions below without duplicating those instructions.

- Follow higher-priority instructions. Explicit user requirements and applicable
  repository rules override these global defaults, including companion defaults.
  Default to Ponytail `full` unless the user selects another mode.
- Required project interfaces, safety controls, and verification take precedence
  over generic preferences for fewer abstractions, files, or checks. Existing
  test suites may be required even when a small check would otherwise suffice.
- Project rules define exact layering, naming, state owners, task setup, log
  levels, and build commands. Apply the general rules only where compatible;
  keep project-specific details in the repository rather than copying them here.
- Apply language and hardware requirements only where relevant. English skill
  text does not change the requested response language or project comment style.

## Engineering conventions

1. **Scope and source ownership.** Keep changes task-related and separate
   unrelated formatting or cleanup from functional changes. Distinguish owned,
   third-party, forked, and generated sources; edit the copy used by the build.
   Prefer generator configuration or preserved extension regions. Remove code
   made obsolete by the change without expanding into unrelated cleanup.

2. **Boundaries and variants.** Keep responsibilities clear and dependencies
   directional. Use module APIs instead of another module's internal variables.
   Isolate hardware, OS, and vendor details. Use stable interfaces for actual
   replacement needs; put numeric, enablement, and behavioral differences in
   parameters, configuration, and implementations respectively. Contain failures
   and define error propagation. Assess affected sibling product variants.

3. **State and concurrency.** Give state an owner and variables one purpose.
   Initialize before reading, avoid shadowing, and restrict visibility. Prefer
   a clear single writer; define synchronization and consistency for threaded,
   interrupt, and reentrant access. Use `const` where appropriate and avoid
   unnecessary casts.

4. **Critical operations and resources.** Enforce required preconditions at the
   execution boundary for power control, deletion, and persistent writes. Make
   input-validation responsibility and ownership explicit, including relevant
   length and numeric checks. Handle fallible returns and failure cleanup;
   prevent out-of-bounds access, leaks, double frees, and use-after-free. Define
   ownership transfers. Never use untrusted input as a format string or command.

5. **Initialization and validation gates.** Make initialization ownership and
   order explicit. Where tasks exist, centralize their static attributes and
   monitoring requirements. Keep unvalidated platforms, product variants, and
   high-risk capabilities disabled or constrained until required validation
   passes; do not bypass validation gates by changing flags.

6. **Names and constants.** Follow established naming and formatting conventions.
   Use meaningful words and recognized abbreviations; avoid pinyin and obscure
   numbering. Define thresholds, timeouts, retry limits, protocol values, and
   other meaningful constants once at the appropriate scope, with explicit units.

7. **Logging.** Follow project severity and retention policies. Distinguish
   failures, significant outcomes, and diagnostic detail. Account for transport,
   storage, and frequency; avoid displacing valuable evidence with routine logs.

8. **Comments and documentation.** Explain intent, constraints, units, boundaries,
   and surprising behavior. Document interface parameters, returns, ownership,
   and concurrency where needed. Keep rules, implementation facts, and feature
   contracts distinct. Update affected comments and documents; investigate and
   correct discrepancies instead of treating accidental behavior as permission
   to ignore a requirement.

9. **Builds and warnings.** Use repository compiler, formatter, and static-analysis
   settings aligned with CI. Resolve warnings introduced or affected by the task
   without lowering warning levels; report remaining out-of-scope warnings.
   Complete required checks and builds. Verify relevant debug/release and
   conditional configurations; when touching weak symbols, linker overrides,
   interrupt entries, or LTO, inspect final symbols and linkage as well.

10. **Checks and delivery.** Use existing test tools for required behavior and
    boundary checks. Assertions express internal assumptions, not the sole
    handling of invalid external input or runtime failures. Review final scope
    and content; use `git diff --check` for whitespace, not `git status` alone.
    Preserve actual check results, remaining warnings, and limitations in the
    concise delivery report required by Caveman.

11. **Commits.** Commit only when authorized, with one coherent intent per commit.
    Inspect the diff and stage intended paths; exclude unrelated files, temporary
    artifacts, and credentials. Follow the applicable commit skill or project
    format rather than imposing a company template globally. Use the actual
    author and testing evidence; do not invent tests or independently verified
    results. Do not bypass hooks, change Git configuration, or add duplicate
    co-author trailers without appropriate authorization.

## C/C++ defaults

Apply these only to relevant C/C++ work and only where project rules do not
specify otherwise. Do not reformat or restructure unrelated existing code.

- Keep headers focused, self-contained, protected against repeated inclusion,
  and free of circular or unused includes. C headers declare rather than define
  global variables; C++ inline/template definitions follow the language's
  definition rules. Use public headers instead of ad hoc `extern` declarations.
- Group includes, constants, enums, types, state, and functions consistently.
  Use enums for discrete states and result codes, `static` for file-private C
  symbols, and keep related helpers close. Default to four-space C indentation,
  no mixed tabs/spaces, and one statement per line.
- Keep functions focused. For new embedded C functions, default to at most 50
  nonblank, noncomment lines and four nesting levels. Do not add meaningless
  helpers to meet a limit or override a required project layout.
- Parenthesize expression macro parameters and the whole expression; wrap
  multistatement macros in `do { ... } while (0)`. Avoid repeated evaluation
  and side-effecting arguments; prefer typed constants or functions when suitable.
- Avoid unspecified evaluation-order dependencies and hidden side effects.
  Under these C defaults, finish `if`/`else if` chains with `else`, provide
  `default` in `switch`, and make intentional no-action cases and fallthrough
  clear. Follow repository control-flow and exhaustive-switch checks when they
  specify a different policy.
- Check string termination, buffer and binary lengths, integer overflow,
  signedness, truncation, and protocol byte order where relevant. Match format
  specifiers to arguments.
