# PostingPlugin — Claude Code Plugin

PostingPlugin is a Claude Code plugin for researching, composing, editing, and fact-checking blog posts. A **chairman** coordinator agent orchestrates four specialist sub-agents through the full pipeline and iterates until the post is publish-ready.

## Slash command

| Command | File | Description |
|---|---|---|
| `/write-post [topic]` | `.claude/commands/write-post.md` | Start the full chairman pipeline for a new blog post |

Use `/write-post` to kick off a new post. Supply the topic inline or leave it blank and Claude will prompt you for the topic, audience, length, and any special constraints before invoking the chairman agent.

## Sub-agents

| Agent | File | Responsibility |
|---|---|---|
| **Chairman** | `.claude/agents/chairman.md` | Coordinate the full pipeline and iterate until the post is publish-ready |
| **Researcher** | `.claude/agents/researcher.md` | Gather background information and key facts on a topic |
| **Composer** | `.claude/agents/composer.md` | Write the first draft from the research brief |
| **Editor** | `.claude/agents/editor.md` | Refine structure, tone, clarity, and SEO |
| **Fact Checker** | `.claude/agents/fact-checker.md` | Verify every factual claim in the draft |

## Coordinated workflow

For a new blog post, invoke the `chairman` agent. It will automatically run the full pipeline and iterate until the output is satisfactory.

The chairman orchestrates the agents in this order, looping stages 2–5 as needed:

1. **Research** — Use the `researcher` agent to gather information on the topic. Pass the topic and any constraints (audience, length, angle) to the agent. It returns a structured research brief.
2. **Compose** — Pass the research brief to the `composer` agent. It returns a full first-draft post (title, introduction, body sections, conclusion, metadata suggestions).
3. **Fact-check (draft)** — Pass the draft to the `fact-checker` agent. It returns an annotated list of claims with a verdict (verified / unverified / false) and suggested corrections.
4. **Edit** — Pass the draft together with the fact-checker's corrections to the `editor` agent. It returns the polished post.
5. **Fact-check (final)** — Pass the editor's output back to the `fact-checker` agent to ensure no new or altered claims slipped through during editing.
6. **Quality gate** — The chairman evaluates the final fact-check report. If the post contains false or unresolved unverified claims, or outstanding editorial issues, it sends the draft back to the `composer` with targeted revision instructions and repeats steps 2–5 (up to three iterations total).

Agents may be invoked individually when only one stage is needed (e.g. re-editing an existing post, or fact-checking content written elsewhere).

## Inputs and outputs

Each agent accepts plain text or markdown. Keep inter-agent handoffs as markdown so that structure is preserved through the pipeline.

## Usage examples

```
/write-post The history of the Linux kernel for intermediate developers
```

```
Fact-check this draft article: [paste draft]
```

```
Edit this post for clarity and SEO: [paste draft]
```
