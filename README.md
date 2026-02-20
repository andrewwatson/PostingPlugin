# PostingPlugin

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that manages the full lifecycle of blog post creation through four coordinated sub-agents: **Researcher**, **Composer**, **Fact Checker**, and **Editor**.

## Quick start

1. Install [Claude Code](https://docs.anthropic.com/en/docs/claude-code).
2. Clone this repository into your project or add it as a Claude Code plugin.
3. Ask Claude to write a blog post — it will automatically coordinate the four agents to research the topic, write a draft, verify every fact, and polish the final copy.

```
Write a 1000-word blog post about the history of the Linux kernel aimed at intermediate developers.
```

## Plugin structure

```
PostingPlugin/
├── CLAUDE.md                        # Main plugin instructions and workflow
└── .claude/
    └── agents/
        ├── researcher.md            # Gathers facts and builds a research brief
        ├── composer.md              # Writes the first draft from the brief
        ├── fact-checker.md          # Verifies every claim in the draft
        └── editor.md                # Polishes the post and applies corrections
```

## Coordinated pipeline

```
User request
     │
     ▼
 researcher ──► composer ──► fact-checker ──► editor ──► Final post
```

1. **Researcher** — searches the web and builds a structured brief (key facts, context, outline, sources).
2. **Composer** — turns the brief into a full first draft (title, sections, meta description).
3. **Fact Checker** — verifies every factual claim and returns a report of verdicts and corrections.
4. **Editor** — applies corrections and polishes the post for clarity, tone, grammar, and SEO.

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
