# Agentic AI Production Checklist

A practical checklist for teams moving agentic AI workflows from demo to production.

Most teams can get an agent demo working. The hard part is making it **safe, auditable, and predictable** under real load. This checklist covers the production failures we see most often — and what to do about them.

> Assumption: your agent can call tools (APIs), retrieve data (RAG), or take actions. If it's purely a chat UI with no actions, you can skip the tool guardrail sections.

---

## 1. Control Flow & Guardrails

If you can't control what the agent is allowed to do, you don't have a system — you have a liability.

- [ ] **Tool allowlist** — only approved tools exposed, with explicit schemas and required parameters
- [ ] **Least privilege** — scoped tokens per tool and per environment (prod vs sandbox)
- [ ] **Loop limits** — cap retries and iterations; detect and break runaway behaviour
- [ ] **Idempotency** — any external action (write, send, charge) is safe to retry without side effects
- [ ] **Human-in-the-loop** — review step for high-risk actions (refunds, deletes, approvals)
- [ ] **Kill switch** — feature flag + safe fallback path when the agent misbehaves

---

## 2. Retrieval Correctness (RAG)

Bad retrieval creates confident wrong answers. Fix retrieval first, then tune prompts.

- [ ] **Grounding rule** — require citations or evidence snippets for key claims
- [ ] **Golden set** — 30–100 real questions with expected supporting documents
- [ ] **Eval loop** — measure answer correctness and faithfulness on every change
- [ ] **Freshness & drift** — define what must be up-to-date; schedule re-indexing and validation
- [ ] **PII boundaries** — prevent retrieval of sensitive fields unless explicitly required

---

## 3. Observability & Debuggability

If you can't explain why it did something, you can't operate it.

- [ ] **Trace every run** — inputs (redacted), tool calls, outputs, decision checkpoints
- [ ] **Version everything** — prompt versions, tool schemas, retrieval config, model config
- [ ] **Failure taxonomy** — classify failures: retrieval miss, tool error, policy block, timeout
- [ ] **Redaction** — log safely; never store secrets or sensitive raw content in traces

---

## 4. Cost & Latency Budgets

An agent without budgets will surprise you — and then it will get turned off.

- [ ] **Budget per request** — cap tokens, tool calls, and wall-clock time per run
- [ ] **Graceful fallback** — when budget exceeded, degrade to a simpler flow (not a hard failure)
- [ ] **Caching** — cache retrieval + tool responses where safe; invalidate correctly
- [ ] **Batching** — reduce tool chatter; avoid redundant calls to expensive APIs

---

## 5. Pre-Launch Sanity Check

Before going live, verify:

- [ ] Agent has been tested against adversarial inputs (prompt injection, jailbreak attempts)
- [ ] All tools have been tested in isolation before wiring into the agent
- [ ] Cost and latency benchmarked on realistic load (not just happy-path single requests)
- [ ] Rollback plan exists — you can disable the agent and fall back to a non-agentic flow
- [ ] At least one person on the team can read and explain a full agent trace end-to-end

---

## How to Use This

**As a pre-launch gate** — run through before shipping any agent to production. Treat unchecked items as known risks, not skipped steps.

**As a review tool** — use in architecture reviews or team retrospectives to identify gaps in existing agentic systems.

**As a shared artifact** — share with your team or stakeholders to align on what "production-ready" means for your agent.

---

## Related Resources

- 📄 [Full article: Agentic AI in Production — the Reliability Checklist](https://infrixo.com/insights/agentic-ai-production-readiness) — detailed rationale behind each item
- 📄 [Guardrails Over Prompts: Tool Permissions and Human-in-the-Loop](https://infrixo.com/insights/guardrails-over-prompts) — deeper dive on control flow
- 📄 [Observability for Agents: What to Log](https://infrixo.com/insights/agent-observability-what-to-log) — detailed logging and tracing guide
- 📄 [Latency and Cost Budgets for LLM Workflows](https://infrixo.com/insights/llm-latency-cost-budgets) — budgeting patterns in depth

---

## About

Maintained by [Infrixo Systems](https://infrixo.com) — boutique technology advisory for architecture, scaling, and agentic AI in production.

If you're moving an agent prototype to production and want a second opinion on your stack, start with a free [Foundation Check](https://infrixo.com/start).

---

*Found this useful? Star the repo — it helps others find it.*
