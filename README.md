# PostingPlugin

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that manages the full lifecycle of blog post creation through five coordinated sub-agents: **Chairman**, **Researcher**, **Composer**, **Fact Checker**, and **Editor**.

## Installation

Requires Claude Code 1.0.33 or later.

```bash
# Install from GitHub (project scope — shared with your team via .claude/settings.json)
claude plugin install andrewwatson/PostingPlugin --scope project

# Or install for your user account (available in all projects)
claude plugin install andrewwatson/PostingPlugin --scope user
```

To test locally without installing:

```bash
claude --plugin-dir /path/to/PostingPlugin
```

## Quick start

Once installed, use the `/posting-plugin:write-post` slash command to kick off the full pipeline:

```
/posting-plugin:write-post The history of the Linux kernel for intermediate developers
```

Claude will prompt you for any missing details (audience, length, angle) and then run the full pipeline automatically.

## Plugin structure

```
PostingPlugin/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── commands/
│   └── write-post.md            # /posting-plugin:write-post slash command
├── agents/
│   ├── chairman.md              # Coordinates the pipeline and iterates until publish-ready
│   ├── researcher.md            # Gathers facts and builds a research brief
│   ├── composer.md              # Writes the first draft from the brief
│   ├── fact-checker.md          # Verifies every claim in the draft
│   └── editor.md                # Polishes the post and applies corrections
└── CLAUDE.md                    # Plugin instructions loaded into every session
```

## Namespacing

Because this is a plugin, all commands and agents are namespaced with `posting-plugin:`:

| Resource | Name |
|---|---|
| Slash command | `/posting-plugin:write-post` |
| Chairman agent | `posting-plugin:chairman` |
| Researcher agent | `posting-plugin:researcher` |
| Composer agent | `posting-plugin:composer` |
| Fact-checker agent | `posting-plugin:fact-checker` |
| Editor agent | `posting-plugin:editor` |

## Coordinated pipeline

The `chairman` agent runs the full pipeline and loops the compose → fact-check → edit cycle until the post meets quality standards (or up to three revision iterations).

```
User request
     │
     ▼
 chairman
     │
     ▼
 researcher ──► composer ──► fact-checker ──► editor
                    ▲                              │
                    │         (loop if needed)     │
                    └──────────────────────────────┘
                                                   │
                                                   ▼
                                            Final post + Chairman's summary
```

1. **Chairman** — coordinates all agents, evaluates quality, and iterates until the post is publish-ready.
2. **Researcher** — searches the web and builds a structured brief (key facts, context, outline, sources).
3. **Composer** — turns the brief into a full first draft (title, sections, meta description).
4. **Fact Checker** — verifies every factual claim and returns a report of verdicts and corrections.
5. **Editor** — applies corrections and polishes the post for clarity, tone, grammar, and SEO.

## Individual agents

Agents can also be invoked individually when only one stage is needed:

```
# Edit an existing post
Edit this post for clarity and SEO: [paste post]

# Fact-check standalone content
Fact-check this article: [paste article]

# Research only
Research the current state of WebAssembly for a technical audience.
```

## Requirements

- Claude Code 1.0.33 or later (plugin system public beta)
- Web search tool enabled (required by the Researcher and Fact Checker agents)
