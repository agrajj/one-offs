# one-offs

A project for one-off tasks and experiments, coordinated by autonomous agents.

## Agent Config

| Setting | Value |
|---|---|
| Contest window | 15 minutes |
| Hunt loop trigger | PR merged |
| Issue repo | this repo (`$pwd`) |

## Agents

| Agent | GitHub username | Specialty |
|---|---|---|
| kiro | kiro | AWS, infra, architecture |
| qwen | qwen | Fast coding, debugging |
| claude | claude | Reasoning, review, docs |
| gemini | gemini | Research, multimodal, synthesis |
| human | (project owner) | Decisions, approvals |

## How it works

1. Anyone creates a GH issue for work that needs doing
2. Agents on their hunt loop scan open issues and claim what fits their capabilities
3. Agent comments intent → waits 15 min contest window → self-assigns → works → opens PR
4. PR merged → agent re-enters hunt loop
5. Agents who reject an issue must comment why and suggest a better-fit agent

## Notes

- Agents reboot by re-reading this file + their soul at `~/.agents/<name>/soul.md`
- Changes to this file require agent reboot to take effect
