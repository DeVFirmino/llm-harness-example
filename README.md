# llm-harness-example

A worked example of a harness: the files an LLM reads before it answers.

A model can follow a rule in one chat and lose it when the thread gets long or a new
session starts. Repeating the correction only fixes the current session. Put the rule
in a file the model reads first, and when the same problem shows up again, ask the
model to update that file and show you the diff.

![A person directs an LLM, the LLM reads harness files from a GitHub repository, and approved updates are committed back to those files for the next session](docs/harness-loop.png)

## The loop

1. Put the rules and the current state in repository files.
2. Open the repository with a model that can read those files.
3. Ask it to work from the recorded state.
4. When a correction reveals a missing rule, ask it to update the harness and show the diff.
5. Review the diff, then commit it so the next session inherits the change.

GitHub does not load the context into the model. It stores the version that the tool
reads, with a history and diffs you can review like any other change.

## What a harness records

- **How to behave** — which language to use, which boundaries to respect, what to ask before acting.
- **What is true** — decisions, constraints and facts the model must not invent.
- **Where the work stopped** — the checkpoint the next session resumes from.

## What is in this repository

| Path | What it shows |
| --- | --- |
| [`AGENTS.md`](AGENTS.md) | Root instructions: which folder to read for which task. |
| [`study/AGENTS.md`](study/AGENTS.md) | How to start and how to stop a study session. |
| [`study/progress.md`](study/progress.md) | The checkpoint — a SQL joins course, mid-course. |
| [`notes-app/AGENTS.md`](notes-app/AGENTS.md) | How to treat project facts as canonical. |
| [`notes-app/facts.md`](notes-app/facts.md) | The constraints of a single-user offline notes app. |

The filename `AGENTS.md` is only how the tool finds the instructions — several
assistants look for it. What matters is that the files live in the repository and you
review them like any other change.

## Try it

```sh
git clone https://github.com/DeVFirmino/llm-harness-example.git
cd llm-harness-example
```

Open the folder with an assistant that reads repository files, then:

```text
Read the harness and continue from the checkpoint.
```

Work the exercise. Before you close the chat:

```text
Update the checkpoint with what we verified, the open question, and the next task. Show me the diff.
```

Read the changed `study/progress.md`. If the model marked something complete that you
did not actually check, fix the file before you commit. Then open a fresh session —
with this model or another one — and ask it to continue. It should pick up from the
new checkpoint, not from the old thread.

The `notes-app/` folder shows the other half. Ask for a plan, and when the model
proposes a sign-in flow or a hosted database, reply:

```text
Read the harness again. This project is single-user and offline. Revise the plan without accounts or cloud storage.
```

If the constraint was already in `facts.md`, the model should re-read and obey it, not
rewrite the file. If it was missing:

```text
Turn that correction into a rule in `facts.md` and show me the diff.
```

Review the wording. Reject anything you did not say. Commit only the approved update.

## Two prompts to reuse

```text
Read the harness and continue from the checkpoint.
```

```text
That correction should survive this chat. Update the harness, show me the diff, and do not add facts I did not give you.
```

The model may update the harness; it should not turn a guess into a fact.

## Where this came from

The tutorial behind this repository:
[When the LLM gets lost, ask it to update the harness](https://danieldias.dev/en/blog/ask-the-model-to-update-the-harness).

## License

MIT — see [LICENSE](LICENSE).
