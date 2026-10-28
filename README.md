<h1 align="center">Cross-Session Coordination for Codex</h1>

<p align="center">
  <strong>Relevant information reaches the owner who can act on it.</strong>
</p>

<p align="center">
  <a href="skills/cross-session-coordination/SKILL.md">
    <img alt="Codex Skill" src="https://img.shields.io/badge/Codex-Skill-2563eb?style=flat-square">
  </a>
  <a href="LICENSE">
    <img alt="MIT License" src="https://img.shields.io/badge/license-MIT-0f8f88?style=flat-square">
  </a>
</p>

<p align="center">
  <a href="#when-it-helps">Use cases</a>
  &middot;
  <a href="#install">Install</a>
  &middot;
  <a href="README.zh-CN.md">中文</a>
  &middot;
  <a href="skills/cross-session-coordination/SKILL.md">Skill Reference</a>
</p>

This repository provides the `cross-session-coordination` Codex skill. It helps independently owned sessions communicate findings, resolve dependencies, and hand work over with clear responsibility. A coordinator can handle shared decisions and conflicts while individual owners communicate directly.

## When It Helps

- A producer changes behavior that another session consumes.
- A consumer needs an interface or dependency owned elsewhere.
- A checker finds a semantic mismatch, or a producer discovers missing checker coverage.
- One session solves a blocker that another session is actively facing.
- An owner needs to hand unfinished work to another owner.
- A stage is complete and needs a decision about the next objective.

Messages explain the problem, impact, requested action, and evidence location in a few readable sentences. Detailed logs and technical identifiers stay in linked evidence when they are not needed to make the immediate decision.

## How It Works

| Principle | Expected behavior |
| --- | --- |
| Each workstream has an owner | Responsibility stays local; a handoff needs the successor's acceptance |
| Communicate where action is needed | Directly affected owners exchange relevant information without routine broadcasts |
| Confirmation has meaning | Delivery, acceptance, work delivery, and verification remain distinct |
| Follow up proportionately | Avoid repeated pending requests and acknowledgment loops; escalate unresolved boundaries |
| Keep evidence recoverable | Reuse existing conversations and project records without imposing a storage format |

## Install

The example uses `~/.codex/skills` (or `CODEX_HOME`), as supported by this installation. If your client uses a different user skill directory, change the destination accordingly.

```bash
git clone https://github.com/xukp20/codex-cross-session-coordination.git
cd codex-cross-session-coordination
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cross_session_skill_target="${CODEX_HOME:-$HOME/.codex}/skills/cross-session-coordination"
if [ -e "$cross_session_skill_target" ] || [ -L "$cross_session_skill_target" ]; then
  echo "Destination already exists; inspect it before changing the installation."
else
  ln -s "$PWD/skills/cross-session-coordination" "$cross_session_skill_target"
fi
```

Reload Codex so it can discover the skill. The link follows this checkout; update it with `git pull --ff-only` from the repository. The installation example preserves any existing destination.

## Use

```text
Use $cross-session-coordination to review the dependency between these sessions.
Identify which owner needs to act, and prepare a short message with the impact
and evidence link. I will review the message before it is sent.
```

```text
Use $cross-session-coordination for these related workstreams. You may message
their existing owners about dependencies and findings that change their work.
Escalate shared decisions to me; keep routine progress in each session.
```

Normal automatic discovery is enabled for relevant tasks. Explicit invocation also works. Skill selection does not grant permission to send messages, create tasks, or change another repository; the user's existing authorization determines which actions can proceed.

## How It Differs from Single-Session Delegation

Single-session delegation assigns bounded work to subagents within a parent session. This skill concerns independent sessions whose owners retain their own objectives, context, and permissions. It does not choose models, launch workers, or manage an internal agent tree, and it does not require every finding to pass through a top coordinator.

## Boundaries

The skill is communication guidance, not a mailbox service or task manager. It requires no particular app or messaging API and provides no background listener, delivery guarantee, timer, or automatic wakeup. Use a suitable authorized channel when one is available; otherwise prepare a handoff and report what remains unsent.

It does not prescribe a shared directory, database, event schema, status workflow, or global backlog. Existing project conventions remain in charge of records and evidence.

## Validation

Use the validator bundled with Codex's `skill-creator` system skill:

```bash
python /path/to/skill-creator/scripts/quick_validate.py \
  skills/cross-session-coordination
```

This checks the skill's structure and metadata; behavioral review is separate.

## Sources and Influences

The instructions are independently written. These primary sources informed the communication boundaries; none is a runtime dependency:

- [MCP Agent Mail Rust](https://github.com/Dicklesworthstone/mcp_agent_mail_rust): persistent identities, directed messages, acknowledgments, and recoverable evidence.
- [Claude Code cross-session messaging](https://code.claude.com/docs/en/cross-session-messaging): independent-session communication and recipient permission boundaries.
- [OpenAI Agents SDK handoffs](https://github.com/openai/openai-agents-python/blob/main/docs/handoffs.md): explicit handoff destinations and control over transferred context.

Released under the [MIT License](LICENSE).
