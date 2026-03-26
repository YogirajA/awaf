# AWAF Framework

Full pillar definitions for the AI Agents Well-Architected Framework.

---

## Foundation (Prerequisite)

**Vertical Slice & Autonomy**: Agents must own their domain end-to-end with independent tools, context, and data. A vertically sliced agent owns its domain end-to-end: its tools, its context, its data. Tools must be single-purpose with explicitly described capabilities. Inter-agent data contracts must be typed rather than free-form text.

---

## Six Cloud-Adapted Pillars

### 1. Operational Excellence

Instruments all other pillars. Requires SLOs, playbooks, and postmortems. Determines whether the other pillars remain effective in production. Focuses on consistent uptime and system visibility.

### 2. Security

Enforced in code, not prompts. Uses trust tiers at the MCP (Model Context Protocol) layer. Credentials must never enter the agent. Blast radius must be explicitly bounded.

> "Don't do X" in system prompts is merely a suggestion.

### 3. Reliability

Designed for failure, not just uptime. Chain boundaries function as fault domains. Requires fail-loud behavior and circuit breakers at the MCP layer. Implements checkpoint/resume for multi-step runs to prevent silent, confident failures.

### 4. Performance Efficiency

Optimizes execution speed and resource usage across agent operations, adapted from cloud infrastructure principles.

### 5. Cost Optimization

Tracks every token and tool call. Implements session budgets and loop detection from day one. Hard stop at 100% budget. Non-negotiable. Prevents solutions that cost more than the problems they solve.

### 6. Sustainability

Long-term viability and environmental considerations, adapted from cloud WAF principles.

---

## Three Agent-Native Pillars

### 7. Reasoning Integrity

Addresses silent, confident failures, the worst failure type. Agents can hallucinate arguments, select wrong tools, or derail without visible errors. Requires evals covering tool selection, argument accuracy, and chain-of-thought faithfulness. MCP should provide provenance metadata on every response.

### 8. Controllability

Maintains human control through code-level enforcement, not prompts. Implements trust tiers at the MCP layer. Any in-flight agent must be externally stoppable. Provides pause, notify, and resume/abort primitives.

### 9. Context Integrity

Manages agent perception of reality. Prevents stale context from corrupting reasoning. Requires external content sanitization through MCP. Enforces active lifecycle management for long sessions. The agent must understand its own knowledge limitations. Agent state must be explicitly persisted during long sessions (scratchpad, memory store, or equivalent) so reasoning survives context degradation and crash-resume cycles. Tool response outputs must be filtered to relevant fields before re-entering context; verbose responses are a context corruption risk.

---

## Scoring Methodology

Each pillar uses a mechanical risk tally. Every criterion is assigned H (3 pts), M (2 pts), or L (1 pt) and marked pass or fail with a one-line evidence citation. The pillar score equals `round(sum_passed_pts / sum_all_pts × 100)`.

The overall score is a weighted average: `sum(score × weight) / sum(weights)`. Tier 2 pillars carry 1.5× weight. Skipped or not-applicable pillars are excluded from both numerator and denominator.

---

## Implementation Principle

These pillars are **concurrent, not sequential**. They function as load-bearing walls, not sequential steps. The vertical slice foundation must be established first.

Source: [aradhye.com/ai-agents-well-architected-framework](https://aradhye.com/ai-agents-well-architected-framework/)
