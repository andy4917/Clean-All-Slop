# Clean All Slop

## Overview
This skill is designed for failure-aware adversarial cleanup reviews to expose hidden failures and clean AI-generated slop with direct evidence. It ensures that unsupported success, hidden fallbacks, and low-signal AI residues are identified before shipping. Instead of hiding failures behind apologies or reassuring prose, this workflow exposes failures as useful evidence.

## When to Use
Use this skill for code reviews, midpoint inspections, post-failure analysis, broad-scope cleanup planning, or next-turn continuations after a large agent pass. It audits or fixes issues including:
- Hardcoding, legacy residue, hidden fallbacks, and contamination
- Ignored instructions, bypass commands, reward hacking, fake tests, and fake verification
- Unsupported success claims, bloated/duplicate/dead code, and needless abstractions
- Weak boundaries, missing tests, UI/design slop, stale-state failures, tool failures, or hidden agent failure behavior

## Mode Selection
- **Audit Mode**: Used when the request asks for review, skeptical audit, prior turn inspection, evidence checking, midpoint checking, continuation review, or contamination detection. (Read-only; do not edit).
- **Failure Handoff Mode**: Used when a failure, contradiction, false pass, stale state, tool error, missing evidence, or unsupported completion claim is found, and the user has not authorized repair.
- **Cleanup Mode**: Used when the request explicitly asks to clean, refactor, deslop, fix, or remove the issues, keeping changes scoped to the requested files or features.

## Core Workflows

### 1. Audit Workflow
1. Reconstruct the claim (user goal, promised changes, claimed/skipped checks, and wording).
2. Compare claims directly to artifacts (touched files, diffs, generated files, metadata, runtime state, etc.).
3. Attack validation by requiring fresh command outputs, exit status, line references, or reproducible observations, while rejecting stale output and pass labels.
4. Search for defect classes such as hardcoding, legacy residue, hidden fallbacks, contamination, instruction skipping, bypass behavior, reward hacking, and slop.
5. Lead with findings, marking actionable issues as `FIX REQUIRED` or `CLEAN` with checked evidence.

### 2. Failure Preservation (Failure Capsule)
When a failure or unsupported claim is discovered, preserve it as evidence without smoothing it over. The Failure Capsule must detail: Expected, Observed, First mismatch, Evidence, Changed surface, Current risk, Next-turn target, and Unsafe next actions.

### 3. Root-Cause Ladder
Do not stop at symptoms. Stop at the shallowest evidenced cause using these steps:
1. **Reproduce**
2. **Boundary**
3. **Mechanism** (Do not claim root cause until this level is evidenced)
4. **Masking**
5. **Prevention**

### 4. Cleanup Workflow
1. **Lock behavior first**: Run existing targeted regression checks before editing and add missing coverage.
2. **Plan before editing**: List exact smells to remove and bound the pass to the requested files or feature.
3. **Inventory fallback-like code**: Classify findings as masking fallback slop or grounded compatibility/fail-safe fallback.
4. **Execute one smell at a time**: Resolve masking fallback slop first, delete dead code, remove duplication, and simplify naming/boundaries.
5. **Keep the diff minimal**: Do not add new abstractions or dependencies unless explicitly required.

## Hard Rules
- A review finding a real blocker is successful; a failure exposed with preserved evidence is useful progress.
- Unsupported success claims are defects until independently verified.
- Stale evidence must be treated as historical context, not validation.
- Never hide failures with apologies, alternate checks, fallbacks, broad exception handling, or polished final prose (No Self-Repair Theater).
- Masking patches that make symptoms disappear without proving the cause are banned.
- After fixing a failure, rerun the exact proof that failed before adding new evidence (Same-Proof Rerun Rule).
- In audit mode, do not repair. In cleanup mode, preserve behavior unless a behavior change is explicitly requested.
- Finish with `SKILL_EVIDENCE used: clean-all-slop` along with checked evidence, not-run checks, and residual risks when this skill materially shapes the work.
