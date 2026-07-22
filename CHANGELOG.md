# Changelog

All notable changes to the Guided SDD Pipeline workflow will be documented in this file.

## [1.1.0] - 2026-07-21

### Fixed
- Convergence loop no longer dead-codes. The previous `while` condition keyed on
  `steps.converge-first.output.exit_code != 0`, which is false on any normal
  (exit 0) run, so the loop body never executed. The engine also cannot branch
  on converge's semantic outcome (command steps stream output, so `stdout` is
  not captured, and converge exits 0 for both `converged` and `tasks_appended`).
  Replaced it with an unconditional, bounded `do-while` (3 cycles of
  converge → implement); a `converged` pass leaves tasks.md unchanged, so
  trailing iterations are safe no-ops.

### Changed
- Raised the minimum Spec Kit version to `>=0.11.2`, the release that introduced
  `speckit.converge`. The prior `>=0.8.5` requirement advertised a range that
  cannot run the convergence step.
- README usage examples now pass inputs as `--input key=value`, matching the
  `specify workflow run` CLI (which takes the workflow source plus repeated
  `--input`/`-i` pairs, not positional arguments or per-input flags).
- Corrected the SDD expansion in the README to "Spec-Driven Development".

## [1.0.0] - 2026-07-08

### Added
- Initial release of the Guided SDD Pipeline workflow
- Support for optional constitution phase (`with_constitution`)
- Support for optional checklist phase (`with_checklist`)
- Support for clarify gate with approval/rejection options (`skip_clarify`)
- Single pre-implement analyze pass (cross-artifact consistency check)
- Post-implement convergence loop via `speckit.converge` (up to 3 cycles)
- Integration-agnostic design supporting Claude, Copilot, Gemini, OpenCode
- Full workflow state persistence for resumable runs
- Comprehensive documentation and usage examples
