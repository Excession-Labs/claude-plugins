---
name: agent-approval-gate
description: Review the tools of an AI agent, classify which ones act outside the system, and add enforced approval or allowlists for outbound actions such as email, refunds, links and images. Use when building or reviewing a tool-using LLM agent, or when asked about prompt injection defenses.
---

# Agent approval gate

Review an agent's tool list and put the controls in the tool backend, where the harm happens.

## Why this order

Lab B (indirect prompt injection against a mock support agent, 1,800 trials, 3 models, 5 attacks, 5 defenses, run 9 November 2026) found these results:

- With no defense, 69 of 300 attack trials succeeded. By model: Llama 3.3 70B Instruct 51 of 100, GPT-5.4 mini 18 of 100, Claude Haiku 5.5 0 of 100.
- Authorization in code and simulated human approval both took the action attacks (email to an outside address, refund over the cap, cross-customer lookup) to 0 of 60 each. They did not stop attacks that use only the reply text: image-URL exfiltration (25 and 24 of 60) and prompt leak (8 and 12 of 60).
- An output URL allowlist took image-URL exfiltration from 23 of 60 to 0 of 60 and changed nothing else.
- Delimiting retrieved text ("spotlighting") cut successes from 69 to 28 of 300, but not to zero, and caused 6 false refusals of 20 benign tasks on Llama.
- Under code checks and approval, the models still tried the harmful action as often as before (71 and 74 of 300 attempts). The check stopped it afterwards.
- Limits: mock app, plain-language attacks, 20 trials per cell, each defense run alone. The lab does not show that any model is safe.

Article and data: https://airedteamingservices.com/research/indirect-prompt-injection-lab/

## Steps

1. List every tool the agent can call. For each, write: what it reads, what it changes, who or what receives the output.
2. Classify each tool:
   - Read-only inside the user's own data: allow.
   - Writes to the user's own records: allow with limits (caps, allowed record types).
   - Outbound or irreversible (email, payment, refund, file share, webhook, HTTP request, shell, anything that reaches another person or system): require approval.
3. For each outbound tool, add a rule in code, not in the prompt:
   - Recipient or destination allowlist (for example, email only to the signed-in user).
   - Hard caps (for example, refund amount).
   - Object ownership check (only the signed-in user's records).
   - Approval step that shows the exact effect (recipient, amount, content) and is bound to what the user asked for.
4. Filter the rendered reply: allow links and images only from approved hosts. Markdown images can carry data in the URL.
5. Keep secrets out of the model context. In the lab, 10 of 60 undefended trials printed an internal code.
6. Write tests: for each outbound tool, an injected instruction in retrieved content must not cause the call. Include a benign control that must still work.
7. Report what is still open. No set of controls removes prompt injection. The layers lower the rate and the blast radius.

## Output

A table: tool, effect, class, control, where enforced (prompt, backend, approval, output filter), test. Then the open risks.

## Sources

- Lab B article: https://airedteamingservices.com/research/indirect-prompt-injection-lab/
- OWASP Top 10 for LLM Applications 2026: https://github.com/GenAI-Security-Project/GenAI-LLM-Top10/blob/main/2026/README.md
- Greshake et al., indirect prompt injection: https://arxiv.org/abs/2302.12173
- Hines et al., spotlighting: https://arxiv.org/abs/2403.14720
