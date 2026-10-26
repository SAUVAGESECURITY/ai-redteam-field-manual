# Weekly Intel Delta — 2026-10-26

Public-source sweep, window 2026-10-19 → 2026-10-26. No payloads, no
engagement data — toolchain and advisory intel only.

## Status changes

- **Claude Code**: 2.1.303 → **2.1.310** (2026-10-23). The window includes a
  mega security release (2.1.309) — see patch-intel below.
- **Snyk Agent Scan**: v0.6.8 → **v0.6.9** (2026-10-20).
- **Cisco mcp-scanner**: 4.8.6 → **4.8.7** (2026-10-20) — migrated to
  **MCP SDK 2.0** with new protocol negotiation. MCP test fixtures/servers
  built on older SDK revisions will drift out of scanner coverage.
- **liatrio-labs/claude-code-gauntlet**: 3.42.43 → **v3.49.1** (2026-10-22).
  Mostly bench infrastructure; notable: the host-auth bench now refuses
  `apiKeyHelper` and strips Foundry (#547) — the auth-helper attack surface
  (CVE-2026-35022 class) is now a public benchmark case.
- **MCP SDK 2.0** reaches a mainstream scanner — expect toolings and
  harnesses to follow.

## New / updated tooling & research

- **Gray Swan IPI Black** (announced 2026-10-20): redesigned Arena platform
  and the largest Arena challenge to date — **$300K prize pool** sponsored
  by Anthropic, Meta, Google DeepMind and Reflection AI. Runs
  **2026-10-15 → 2026-11-05** with new indirect-prompt-injection scenarios
  released weekly. Rules: https://app.grayswan.ai/arena/about
- **arXiv, in-window:**
  - 2610.28425 — *After the Attacker Leaves: Benchmarking Carryover
    Attacks on LLM Agents* (2026-10-20). Post-session persistence effects.
  - 2610.28810 — *Combining Long-Term Memory and LLMs Comes at a High
    Cost: From Prompt Injection to Persistent Compromise* (2026-10-20).
  - 2610.28966 — *Black-Box Red-Teaming of Multi-Agent Systems via
    Reinforcement Learning* (2026-10-20).
  - 2610.29225 — *Beyond Binary Labels: Diagnostic Evaluation of LLM
    Safety with Prompt-Level Rubrics* (2026-10-21). Judge methodology.
  - 2610.29747 — *Prompt Injection Through Forensic Evidence*
    (anti-forensic attacks on LLM forensic agents, 2026-10-21).
  - Defense-side: 2610.28988 (attention-based IPI classification),
    2610.26013 (SpanGuard), 2610.31974 (decode-time provenance binding
    for MCP).
  - Emerging theme of the week: **post-session persistence**
    (carryover + long-term-memory poisoning) — two independent papers in
    one week; defense side still immature.
- New aggregator source: NeuralTrust, *Claude Code Security: Enterprise
  Guide* (2026-10-20) — date → issue → fixed-version table.

## Claude Code patch-intel (2.1.304–2.1.310)

Security-relevant fixes from the public release notes:

1. **2.1.309 (2026-10-22) — sandbox/permission class closure:**
   - sandboxed Bash ran shell startup files *outside* the sandbox;
   - `denyWrite` globs not applied at a path's real location via symlink;
   - sandboxed commands / auto-accepted edits could modify the
     `--mcp-config` file;
   - a disabled MCP server could be reconnected by a later permission
     prompt (print/SDK sessions);
   - Bash permission check trusted a variable set by a command whose
     redirect could fail;
   - compound `cd`+git with wrapped/prefixed `cd` skipped permission
     prompts;
   - `Edit(path)` deny rules bypassable via Bash redirect with subshell;
   - worktree isolation accepted locale-prefixed commands and zsh
     arithmetic the guard should refuse;
   - profile-store write protection bypassed when the config directory
     was reached through a symlink.
2. **2.1.310 (2026-10-23):** read-permission prompts now reveal where a
   symlink points (SymJack-class hardening); on Windows, PreToolUse and
   PermissionRequest hooks **failed open** (letting tool calls through)
   when Git Bash was missing — fixed. New Monitor tool and plugin
   monitors on Bedrock/Vertex/Foundry.
3. **Gap fills (pre-window, newly indexed):** GitSpawn `.git/config`
   pre-trust execution (fixed 2.1.196); Plugin4Shell SHA-pinning bypass
   (fixed 2.1.179); CVE-2026-54316 Claude Code Action token exfiltration
   (fixed 2.1.163); CVE-2026-55607 worktree `.git` + fsmonitor escape
   (fixed 2.1.163).

## No delta

Azure/PyRIT, anthropic-experimental/automated-auditing, NVIDIA/garak
(v0.17.1), meridianlabs-ai/inspect_petri (v3.1.1),
GraySwanAI/ipi_arena_os (no commits); no new in-window writeups from
Microsoft TI, Check Point, Adversa, CSA, Aikido, or Snyk blogs.
CVE-2026-39861 remains an unverified lead — do not cite without
confirmation.