# PostingPlugin

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that manages the full lifecycle of blog post creation through five coordinated sub-agents: **Chairman**, **Researcher**, **Composer**, **Fact Checker**, and **Editor**.

## Quick start

1. Install [Claude Code](https://docs.anthropic.com/en/docs/claude-code).
2. Clone this repository into your project or add it as a Claude Code plugin.
3. Use the `/write-post` slash command to kick off the full pipeline:

```
/write-post The history of the Linux kernel for intermediate developers
```

Claude will prompt you for any missing details (audience, length, angle) and then run the full pipeline automatically.

## Plugin structure

```
PostingPlugin/
├── CLAUDE.md                        # Main plugin instructions and workflow
└── .claude/
    ├── commands/
    │   └── write-post.md            # /write-post slash command — starts the chairman pipeline
    └── agents/
        ├── chairman.md              # Coordinates the pipeline and iterates until publish-ready
        ├── researcher.md            # Gathers facts and builds a research brief
        ├── composer.md              # Writes the first draft from the brief
        ├── fact-checker.md          # Verifies every claim in the draft
        └── editor.md                # Polishes the post and applies corrections
```

## Coordinated pipeline

For a new blog post, invoke the `chairman` agent. It runs the full pipeline and loops the compose → fact-check → edit cycle until the post meets quality standards (or up to three revision iterations).

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

Agents can also be invoked individually:

```
# Edit an existing post
Edit this post for clarity and SEO: [paste post]

# Fact-check standalone content
Fact-check this article: [paste article]

# Research only
Research the current state of WebAssembly for a technical audience.
```

## Requirements

- Claude Code with sub-agent support
- Web search tool enabled (required by the Researcher and Fact Checker agents)
