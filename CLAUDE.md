# PostingPlugin — Claude Code Plugin

PostingPlugin is a Claude Code plugin for researching, composing, editing, and fact-checking blog posts. Four specialized sub-agents handle each stage and hand off their work to the next, so you get a polished, accurate post from a single request.

## Sub-agents

| Agent | File | Responsibility |
|---|---|---|
| **Researcher** | `.claude/agents/researcher.md` | Gather background information and key facts on a topic |
| **Composer** | `.claude/agents/composer.md` | Write the first draft from the research brief |
| **Editor** | `.claude/agents/editor.md` | Refine structure, tone, clarity, and SEO |
| **Fact Checker** | `.claude/agents/fact-checker.md` | Verify every factual claim in the draft |

## Coordinated workflow

When asked to produce a blog post, run the agents in this order:

1. **Research** — Use the `researcher` agent to gather information on the topic. Pass the topic and any constraints (audience, length, angle) to the agent. It returns a structured research brief.
2. **Compose** — Pass the research brief to the `composer` agent. It returns a full first-draft post (title, introduction, body sections, conclusion, metadata suggestions).
3. **Fact-check** — Pass the draft to the `fact-checker` agent. It returns an annotated list of claims with a verdict (verified / unverified / false) and suggested corrections.
4. **Edit** — Pass the draft together with the fact-checker's corrections to the `editor` agent. It returns the final polished post.

Agents may be invoked individually when only one stage is needed (e.g. re-editing an existing post, or fact-checking content written elsewhere).

## Inputs and outputs

Each agent accepts plain text or markdown. Keep inter-agent handoffs as markdown so that structure is preserved through the pipeline.

## Usage examples

```
Write a 1000-word blog post about the history of the Linux kernel aimed at intermediate developers.
```

```
Fact-check this draft article: [paste draft]
```

```
Edit this post for clarity and SEO: [paste draft]
```
