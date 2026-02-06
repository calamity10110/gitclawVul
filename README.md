![gitclaw banner](banner.jpeg)

Gitclaw is a personal AI assistant that runs entirely through GitHub Actions. Think [OpenClaw](https://github.com/openclaw/openclaw), but self-hosted in your own repository: no servers or extra infrastructure, just GitHub Issues and Actions.

Powered by the [pi coding agent](https://github.com/badlogic/pi-mono), every GitHub issue becomes a persistent chat thread with an AI agent. The repository serves as durable storage: conversation history is committed directly to git, giving the agent long-term memory across sessions. It can search prior context, you can ask it to edit or summarize past conversations, and all changes are versioned.

## How it works

1. **Create an issue** → the agent picks it up, processes your request, and replies as a comment.
2. **Comment on the issue** → the agent resumes the _same session_ with full prior context, like continuing a chat thread.
3. **Everything is committed** → session files, state mappings, and any changes the agent makes are pushed to the repo after every turn.

The agent reacts with 👀 while it's working and removes the reaction when it's done.

### Repo as storage

All state lives in the repo itself:

```
state/
  issues/
    1.json          # maps issue #1 -> its session file
    2.json
  sessions/
    2026-02-04T..._abc123.jsonl    # full conversation for issue #1
    2026-02-04T..._def456.jsonl    # full conversation for issue #2
```

Because sessions are committed to git, the agent can grep and search its own chat history. You can even ask it to edit or summarize past conversations.

## Setup

1. **Fork this repo** — click the "Fork" button on GitHub.
2. **Add your Anthropic API key** — go to your fork's **Settings → Secrets and variables → Actions** and create a repository secret named `ANTHROPIC_API_KEY` with your key.
3. **Open an issue** — the agent starts working automatically.
4. **Comment on the issue** to give follow-up instructions — the agent resumes where it left off.

## Security

By default, the workflow only responds to repository **owners, members, and collaborators**: random users cannot trigger the agent on public repos. This is enforced via the `author_association` check in the workflow.

That said, if you plan to use gitclaw for anything private, **make the repo private**. A public repo means your conversation history is visible to everyone. On the upside, public repos get very generous GitHub Actions usage.

## Configuration

Edit `.github/workflows/agent.yml` to customize:

- **Model:** Add `--provider` and `--model` flags to the `bunx pi` command.
- **Tools:** Restrict with `--tools read,grep,find,ls` for read-only analysis.
- **Thinking:** Add `--thinking high` for harder tasks.
- **Trigger:** Adjust the `on:` block to filter by labels, assignees, etc.

## Acknowledgments

Built on top of [pi-mono](https://github.com/badlogic/pi-mono) by [Mario Zechner](https://github.com/badlogic).

Thanks to [ymichael](https://github.com/ymichael) for nerdsniping me with the idea of an agent that runs in GitHub Actions.
