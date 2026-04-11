---
title: "AI Teammates and the Manager's Dilemma"
description: "Companies are calling AI agents 'teammates' now. What does that actually mean for the people managing the team?"
categories: [Management]
tags: [manager, ai, agents]
---

When Asana announced "AI Teammates" built on [Claude Managed Agents](https://claude.com/blog/claude-managed-agents), the word "teammate" stuck in my head. I've managed engineering teams for a while, and that word means something specific to me. A teammate picks up a task, flags when they're stuck, and tells you in standup that they finished the thing but found a problem with the thing next to it. Calling an AI agent a "teammate" blurs a line I think we should keep sharp.

# The Managed Agent Pitch

Anthropic's [Managed Agents](https://claude.com/blog/claude-managed-agents) platform handles the hard parts of running AI agents in production: sandboxed execution, long-running sessions, credential management, scoped permissions, and tracing. You define what the agent should do, give it tools, and the platform orchestrates the rest.

The real-world examples are compelling. Notion lets teams delegate work to Claude inside their workspace. Rakuten deployed specialist agents across engineering, product, sales, and finance, each one shipped in about a week. Sentry connected their debugging agent to a code-patching agent, so flagged bugs go straight to reviewable PRs.

These are autonomous workers that take tasks, produce deliverables, and operate for hours without human intervention. That's genuinely useful. But it's also why the naming matters.

# Why "Teammate" Makes Me Uncomfortable

Good management is about building trust, creating psychological safety, and helping people grow. None of that maps to an AI agent.

A few things bother me about the framing:

Accountability is asymmetric. When a human teammate ships a bug, there's a conversation. They learn from it. When an AI agent ships a bug, who owns that? The person who wrote the prompt? The platform? The manager who assigned the task?

Growth doesn't apply. A big part of my job is developing people, giving stretch assignments, coaching through failure, building career paths. An AI agent doesn't have a career. You can't promote it. You can't give it feedback that changes its behavior next quarter.

Team dynamics get weird. If half your "team" is agents, what does standup look like? What does sprint planning look like? How do you maintain team cohesion when some members are not people?

I don't think these are reasons to avoid AI agents. They're reasons to be precise about what we're building: tools that do work, not teammates that join a team.

# What Managers Actually Need

I think the better mental model is delegation to a very capable, very literal junior engineer who never gets tired but also never learns.

That reframe changes how you approach it. You need clear specifications, because the agent won't ask clarifying questions the way a human would (or if it does, it's simulating that behavior, not genuinely confused). You need review processes, because the agent's output quality is only as good as your ability to verify it. And you need to decide which tasks are "agent-appropriate," meaning repetitive, well-defined, low-ambiguity work where the cost of a mistake is recoverable.

The best use cases I've seen fit this pattern: document processing, code migration, test generation, data extraction. Work that a human *could* do but shouldn't have to.

# Building an AI Standup Bot

To make this concrete, I built a small demo using the [Claude Agent SDK](https://docs.anthropic.com/en/docs/claude-code/sdk). The idea is an agent that reads a project directory, figures out what changed recently, and generates a standup-style summary. The kind of thing a manager might glance at before a morning meeting.

This is a task where the "AI teammate" framing actually fits, because the agent is doing legwork that saves humans time without pretending to be a person.

## Setup

```bash
pip install claude-agent-sdk
export ANTHROPIC_API_KEY="your-key-here"
```

## The Agent

```python
import anyio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage


STANDUP_PROMPT = """
You are a standup facilitator for a software team. Analyze the current
project directory and produce a concise standup-style summary.

1. Look at recently modified files (find . -name '*.py' -mtime -1
   or check git log for the last 24 hours if this is a git repo).
2. Read the contents of changed files to understand what was worked on.
3. Summarize in this format:

   ## What changed (last 24 hours)
   - [file/area]: brief description

   ## Potential issues
   - Any TODOs, FIXMEs, or obvious problems spotted

   ## Suggested focus for today
   - Based on what you see, the logical next step

Keep it short. A real standup is 60 seconds per person.
"""


async def run_standup(project_path: str):
    options = ClaudeAgentOptions(
        system_prompt="You are a concise, observant engineering lead.",
        allowed_tools=["Read", "Bash", "Glob", "Grep"],
        permission_mode="bypassPermissions",
        cwd=project_path,
    )

    async for message in query(prompt=STANDUP_PROMPT, options=options):
        if isinstance(message, ResultMessage):
            print(message.result)


if __name__ == "__main__":
    import sys

    path = sys.argv[1] if len(sys.argv) > 1 else "."
    anyio.run(run_standup, path)
```

## Running It

```bash
python standup_agent.py /path/to/your/project
```

Point it at any git repo and it'll scan recent changes, read the diffs, and produce something like:

```
## What changed (last 24 hours)
- api/handlers.py: Added rate limiting middleware to the /search endpoint
- tests/test_handlers.py: New test coverage for rate limit edge cases
- config/settings.py: Added RATE_LIMIT_WINDOW and RATE_LIMIT_MAX env vars

## Potential issues
- TODO in handlers.py:42 — rate limit bypass for internal services not implemented yet
- No migration for the new rate_limit_log table referenced in handlers.py

## Suggested focus for today
- Implement the internal service bypass before merging the rate limit PR
- Add the missing database migration
```

## What I Noticed Building This

I didn't write any tool execution code. The SDK handles reading files, running bash commands, and deciding what to look at next. I described the task and the agent figured out the rest. That's the core value of Managed Agents: you skip the infrastructure and go straight to defining behavior.

I limited the tools to `Read`, `Bash`, `Glob`, and `Grep`. No `Edit`, no `Write`. This agent observes and reports. It doesn't change anything. When you're building agents that operate on real codebases, the permission boundary is the most important design decision you make.

My first version produced walls of text. Getting it to "60-second standup" brevity took a few iterations on the prompt. This is what "managing" your AI worker actually looks like in practice: you're not giving performance reviews, you're tuning prompts.

# Where This Leaves Us

AI agents are going to do more of the work that teams currently do. That's already happening. The question for managers is how to integrate them without losing what makes teams work: the trust and shared context that come from people working together.

Use agents for the work. Keep the word "teammate" for the humans. If you're a manager trying to figure out where agents fit, start with the boring, well-defined tasks. That's where they earn their keep.

The [official SDK docs](https://docs.anthropic.com/en/docs/claude-code/sdk) and the [Managed Agents announcement](https://claude.com/blog/claude-managed-agents) are both worth reading if you want to try building something yourself.
