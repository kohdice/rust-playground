# AGENTS.md

## Project Context

Rust learning project organized as a Cargo workspace with members under `crates/*` and `resolver = "3"`.

- `crates/bin-crate`: executable entry point; owns startup and input/output.
- `crates/lib-crate`: reusable logic; may be used by the binary, but must not depend on it.
- Read `Cargo.toml` and `rust-toolchain.toml` before choosing version-dependent syntax or APIs. The current edition is `2024`, and the minimum supported Rust version (MSRV) and toolchain are `1.92`.
- `flake.nix` provides Rust `1.92.0` through `nix develop`. `clippy.toml` currently declares an older MSRV, `1.91.0`; these settings are not synchronized.

## Working Approach

- Keep planning, delegation, and reference loading proportional to the task. Perform small, clear changes directly; read detailed skill references only when the current decision needs them.
- Follow an explicit user plan when provided. Prefer test-driven development (TDD) for testable behavior changes: first demonstrate the failure, then implement the behavior and refactor with passing tests.
- Complete authorized work through verification. Pause only for a requested checkpoint, a blocking decision, or a required permission; identify the instruction or missing information responsible.
- Distinguish existing failures from failures introduced by the change. Investigate their relevance, fix them within the authorized scope, and report anything unresolved.

## Commands and Verification

Run commands from the workspace root. Use `nix develop -c <command>` when the development environment is needed.

| Purpose | Command |
| --- | --- |
| Run the example binary | `cargo run -p bin-crate` |
| Check formatting | `cargo fmt --all -- --check` |
| Check Rust code with Clippy | `cargo clippy --workspace --all-targets -- -D warnings` |
| Build all workspace targets | `cargo build --workspace --all-targets` |
| Run workspace tests | `cargo test --workspace --all-targets` |

- `.github/workflows/ci.yml` defines the four verification commands above. Run them before finishing Rust code or build-configuration changes and before pushing those changes. Keep this list aligned with CI.
- During implementation, use the relevant test scope; follow the active TDD workflow's test tiers when applicable. Report the commands run and their results, including failures or checks not run. Reuse successful results unless subsequent changes or new evidence invalidate them.
- When changing executable examples in Rust documentation comments, also run `cargo test --workspace --doc`.
- For prose-only changes, inspect accuracy, local references, and whitespace. Application tests and agent workflow trials are unnecessary for instruction-only edits.

## Code and Tests

- Follow `rustfmt.toml` and keep Clippy checks free of warnings. Prefer safe Rust and propagate recoverable errors with `Result` and `?`.
- Keep calculations independent of input/output, prefer immutable data, and introduce abstractions only for a concrete need.
- Preserve workspace inheritance for package metadata and shared dependencies. Include the patch component when specifying a new external dependency version.
- Until the version reaches `1.0.0`, backward compatibility can be disregarded: prioritize changing the implementation to match the recommended approach.
- Put unit tests in a `#[cfg(test)] mod tests` block beside the implementation; place integration tests in `crates/<crate>/tests/`. Use descriptive `snake_case` names and test observable behavior and relevant edge cases.
- Document public contracts and explain non-obvious constraints in comments. Keep documentation consistent with behavior; PR descriptions should explain the change and relevant verification.

## Explanations and Sources

- Respond in Japanese; write code, comments, documentation, commits, and PRs in English. Define technical terms before using them and explain the reasoning in language a beginner can follow.
- For ordinary changes, explain what changed, why, and how it was verified. Focus on decisions the diff alone does not explain.
- For teaching requests, explain the mechanism and processing steps with concrete examples. Provide complete runnable examples and line-by-line explanations when requested or useful; include time and space complexity for algorithms and data structures. The `rust-tutor` skill is available when requested.
- Base technical explanations on official documentation and link the specific pages used. Read only the relevant sections of the Rust Book, Rust Reference, standard library documentation, Cargo Book, or official crate documentation on docs.rs.

Relevant references, to consult as needed:

- [Cargo workspaces](https://doc.rust-lang.org/cargo/reference/workspaces.html)
- [Cargo test options](https://doc.rust-lang.org/cargo/commands/cargo-test.html)
- [OpenAI guidance on concise AGENTS.md files](https://learn.chatgpt.com/guides/best-practices#make-guidance-reusable-with-agentsmd)
