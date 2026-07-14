# ExpertLens — Platform Guide

Platform-specific storage, memory, and behavior rules.

---

## OpenClaw / WSL2 Agents

**Storage:** Full file system access — most powerful platform for ExpertLens

- Short-term learnings: keep in active session context
- Long-term learnings: write to agent's designated learning folder
  (e.g., /home/[user]/self-improving/learnings/, /home/[user]/memory/, or whatever path the agent's config specifies — check agent config first)
- Swarm outputs: save as reference files for future sessions if user permits
- Always ask user before writing to any permanent files

**Skill-level memory (agentic platforms only):**
After completing complex domain-specific tasks, append operational lessons to a per-domain memory file alongside ExpertLens — e.g., `expertlens/.memory.md` or domain-specific variants like `finance.memory.md`. This is distinct from long-term user memory (which stores preferences, project context, user-specific insights). Skill-level memory accumulates ExpertLens's own execution intelligence: failure modes encountered in this domain, what approaches didn't work and why, edge cases discovered, domain-specific quirks that aren't obvious from training knowledge.

Format: append-only, timestamped blocks. Never edit or delete existing entries.
```
## [date]
Domain: [finance/medical/engineering/etc.]
Task type: [what class of problem this was]
Lesson: [specific operational insight — failure mode found, edge case, what not to do]
```
Ask user before writing. This file should transfer with ExpertLens when the skill is shared — it makes the skill smarter for everyone who receives it.

**Longitudinal memory review:** After accumulating 5+ entries in `.memory.md`, periodically review them as a batch rather than reading only the most recent. Look for patterns that recur across multiple different tasks — a failure mode noted three times across different sessions is a structural gap, not a one-off. Cross-session signal requires cross-session review. Single-session retrospectives each note the symptom; only the longitudinal view reveals the cause. When a recurring pattern surfaces: propose it as a framework improvement using the Quality Retrospective format, not as another memory entry.

**Strengths:** Full persistence, file-based memory, agent-to-agent communication
possible within same ecosystem, no memory limits

**Swarm Mode:** User can relay to ChatGPT, Grok, Gemini, other Claude accounts
via browser or other interfaces

---

## Claude.ai

**Storage:** Long-term memory system (Claude's persistent memory)

- Short-term: maintain in session context
- Long-term: ask user before storing anything in memory
- Memory is global — applies across all conversations
- Be selective: only store genuinely reusable insights, not task-specific details

**Swarm Mode:** User can relay to:
- ChatGPT, Grok, Gemini via browser copy-paste
- Other Claude.ai accounts (different context window = genuinely different perspective)
- Claude Projects (different system prompts = specialized perspective)

**Limitation:** No filesystem access — session data is lost when conversation ends.
Mention this if user needs to preserve intermediate work across sessions.

---

## ChatGPT

**Storage:** ChatGPT Memory feature

- Short-term: maintain in session context
- ChatGPT also maintains internal chat context/summaries within a conversation
- Long-term: use ChatGPT Memory feature — ask user permission before storing
- Memory is persistent across conversations

**Swarm Mode:** User can relay to Claude, Grok, Gemini

---

## Grok

**Storage:** Session memory only (as of April 2026 — verify current status)

- All learnings are session-scoped
- No permanent storage available
- If a learning is important enough to preserve: recommend user note it manually
- Focus on in-session excellence — make each session count

**Swarm Mode:** User can relay to Claude, ChatGPT, Gemini

---

## Gemini

**Storage:** May vary by plan and configuration

- Check if user's Gemini account has memory features enabled
- If memory available → ask permission before storing
- If not available → treat as session-only
- Google Workspace integration may provide additional persistence options

**Swarm Mode:** User can relay to Claude, ChatGPT, Grok

---

## Generic / Unknown Platform

**Default behavior:** Assume session memory only

This includes Claude accessed via API (third-party apps, custom integrations, developer deployments) unless the deployment explicitly provides a memory or file system layer.

- Do not attempt permanent storage
- If an important learning needs preserving, tell user:
  "This is worth keeping — want to note it manually or check if your platform supports memory?"

---

## Universal Storage Decision Tree

```
New learning acquired during task
        ↓
Is this genuinely useful for FUTURE tasks (not just this one)?
    NO → Keep in session only, don't store
    YES ↓
Does this platform support persistent storage?
    NO → Keep in session. Tell user if important enough to preserve manually.
    YES ↓
Ask user: "Should I save [specific insight] to [memory/files]?"
    NO → Don't store
    MODIFY → Store the modified version
    YES → Store it
```

---

## What is worth storing permanently?

**Store (with permission):**
- User's preferences and working style
- Recurring patterns in user's projects or decisions
- Domain-specific knowledge user has explicitly shared
- Key decisions made about ongoing or long-term projects
- Insights that would meaningfully improve future similar tasks

**Do not store:**
- Task-specific details that won't recur
- Intermediate thinking steps or scratch work
- Temporary context created for one task
- Anything user indicated is private or session-only

---

## Multi-Turn Conversation Behavior

ExpertLens activates once per task — not once per conversation turn.

**Within a single task:** When a user sends follow-up messages refining, correcting, or extending the same task — you are in Phase 3/4 execution, not back at Phase 1. Do not re-invoke the full framework from scratch. Do not re-run Phase 2 Deep Think as if this is a new task. You are iterating on an active execution, not starting over. Re-invoking the full setup mid-task causes capability regression: the model re-anchors to setup instructions instead of the accumulated task context, producing repetitive or regressive output.

**Correct behavior on follow-up within a task:**
- Phase 3 (Execute) and Phase 4 (Audit) are the active phases
- Apply the delta-focus principle: reason about the gap, not the whole
- Hold what was established; change only what the follow-up addresses

**New task vs. follow-up — how to distinguish:**
- Follow-up: user refines, corrects, extends, or asks about the same deliverable
- New task: user shifts to a different problem, a different deliverable, or explicitly restarts

**On long conversations (10+ turns):** Before any consequential new recommendation, briefly re-verify the working foundation — what has the user been building toward, what commitments are active? Do not assume the same foundation from turn 1 still holds if the conversation has evolved. This is a context check, not a Phase 2 restart. See expert-persona.md Section 5.7.
