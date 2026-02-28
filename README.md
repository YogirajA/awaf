# AWAF: Agent Well-Architected Framework

**An open specification for production-ready AI agent architecture.**

[![AWAF Version](https://img.shields.io/badge/AWAF-v1.0-1E3A5F?style=flat-square)](https://github.com/YogirajA/AWAF)
[![License](https://img.shields.io/badge/license-Apache%202.0-green?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](CONTRIBUTING.md)

---

## The Problem

We built microservices wrong. Distributed monoliths, chatty services, coordination nightmares. Now we're building AI agents the same way. Different technology, same architectural sins, and no shared concept of what "production-ready" looks like. Every team inventing their own standards. Every incident teaching the same lessons in isolation.

Cloud infrastructure had this problem in 2012. AWS published the Well-Architected Framework in 2015. The ecosystem aligned around it. AWAF is that specification for AI agents.

---

## What is AWAF?

AWAF is an **open specification**: a versioned standard defining what production-ready AI agent architecture looks like across 10 pillars.

The specification is free, unencumbered, and implementation-agnostic. Any team can adopt it without installing anything. Any tool can implement it. The specification belongs to the community that builds agents, not to any vendor.

The specification defines:
- What questions to ask when evaluating an agent architecture
- How to score answers across 10 structured pillars
- What risk levels map to what scores
- What "Production Ready" vs "High Risk" means, precisely

The reference implementation is [awaf-cli](https://github.com/YogirajA/awaf-cli): a CLI and GitHub Action that scores your agent automatically on every commit.

---

## The 10 Pillars

AWAF is organized in three tiers.

### Tier 0: Foundation (prerequisite)

| Pillar | What It Asks |
|--------|--------------|
| **Vertical Slice and Autonomy** | Can this agent run independently? Does it own its tools, context, and data without depending on other agents? |

The Foundation pillar must pass (score of 40 or higher) before Tier 1 and Tier 2 are evaluated. An agent that cannot run independently has an architectural problem that higher-level pillar scores will only obscure.

A simple gut check: if your agent routinely reaches outside its slice to complete its core task, the slice boundary is wrong.

---

### Tier 1: Cloud WAF Adapted (1.0x weight)

These six pillars adapt the AWS Well-Architected Framework's proven pillars for the agent context.

| Pillar | Core Questions |
|--------|----------------|
| **Operational Excellence** | SLOs defined? Runbooks exist? Postmortem process in place? Monitoring and alerting? |
| **Security** | Trust tiers enforced in code, not prompts? Credentials out of context? Blast radius bounded? Kill switch present? |
| **Reliability** | Fault domain boundaries defined? Fail-loud enforced? Circuit breakers at the tool layer? Checkpoint and resume? |
| **Performance Efficiency** | Right model for the task? Context pruned? Independent subtasks parallelized? Responses cached? |
| **Cost Optimization** | Session budget enforced? Loop detection with hard stop? Token costs tracked? Cost alerts configured? |
| **Sustainability** | Models right-sized? Results cached to avoid redundant LLM calls? Unnecessary tool calls eliminated? |

---

### Tier 2: Agent-Native (1.5x weight)

These three pillars have no cloud equivalent. They exist because agents are not servers.

| Pillar | Why It Has No Cloud Equivalent |
|--------|-------------------------------|
| **Reasoning Integrity** | A server does not hallucinate. Agents do. Evals for tool selection, argument accuracy, chain-of-thought faithfulness, and hallucination rate are not optional. |
| **Controllability** | "Don't do X" in a prompt is a suggestion, not a constraint. Trust tiers, kill switches, and human-in-the-loop checkpoints must be enforced in code. |
| **Context Integrity** | Stale context is corrupted reasoning. External content must be sanitized. Context lifecycle must be managed. Uncertainty must be signable. |

Tier 2 pillars carry 1.5x weight in the overall score because architectural failures here produce failure modes that cloud infrastructure patterns were never designed to handle.

---

## Scoring

### Per-Pillar Score (0 to 100)

Each pillar is evaluated against a set of questions. Each question carries a risk weight:
- High risk: 3 points
- Medium risk: 2 points
- Low risk: 1 point

```
pillar_score = (implemented_weight / total_weight) * 100
```

Answering "none of these apply" on any question triggers an automatic High Risk flag and caps the pillar score at 30.

### Overall AWAF Score

```python
overall = sum(score * (1.5 if tier == 2 else 1.0) for each pillar) /
          sum(1.5 if tier == 2 else 1.0 for each pillar)
```

### Readiness Rating

| Score | Rating | What It Means |
|-------|--------|---------------|
| 90 to 100 | **Production Ready** | Architectural patterns are sound across all pillars |
| 75 to 89 | **Near Ready** | Minor gaps, addressable before production |
| 50 to 74 | **Needs Work** | Meaningful architectural risks present |
| 25 to 49 | **High Risk** | Structural problems that will cause incidents |
| 0 to 24 | **Not Ready** | Do not ship to production |

### Confidence Levels

Every pillar score carries a confidence level based on available evidence:

| Confidence | Meaning |
|------------|---------|
| `verified` | Artifacts were present and analyzed. Score reflects evidence. |
| `partial` | Some artifacts present, some inaccessible (e.g. cloud configs not in repo). |
| `self_reported` | No artifacts found. Score based on absence of evidence. |

A `verified` score of 65 is more meaningful than a `self_reported` score of 90. Confidence must be displayed alongside every score.

---

## What AWAF Covers vs What It Does Not

AWAF performs static architectural analysis based on what is in the repository.

**Can verify:**
- Code-level enforcement patterns (trust tiers, loop detection, kill switch implementation)
- Architectural structure (slice boundaries, agent dependencies, import patterns)
- Documentation coverage (runbooks, SLOs, eval framework presence)
- Cost patterns in code (budget guards, hard stops, token tracking logic)

**Cannot verify (requires live data or cloud access):**
- Whether SLOs are actually being met in production
- Whether evals are passing and what the hallucination rate is
- Cloud resource configurations (IAM roles, actual token spend, alert thresholds)
- Whether circuit breakers are firing under load

When AWAF cannot verify something, it flags it explicitly as `partial` confidence with a note on what is missing. A score with explicit coverage gaps is more trustworthy than a confident score built on assumptions.

---

## Adopting the Specification

### Without any tooling

Read [FRAMEWORK.md](FRAMEWORK.md) for the complete pillar definitions, scoring questions, and evaluation methodology. Run manual architecture reviews against the spec. Cite it in your architecture documentation.

No account, no install, no vendor relationship required.

When citing AWAF, always reference the version: _"This agent is designed to AWAF v1.0."_

### With the reference implementation

[awaf-cli](https://github.com/YogirajA/awaf-cli) implements AWAF v1.0 as a CLI tool and GitHub Action. It runs on every PR that touches agent code and produces a scored report with specific cited findings.

```bash
pip install awaf
awaf run
```

### Building your own implementation

Any tool can implement the AWAF specification. If you build an AWAF-compatible implementation, open a PR to add it to [IMPLEMENTATIONS.md](IMPLEMENTATIONS.md).

---

## Specification Versioning

AWAF is versioned independently of any tool that implements it.

| Version | Status | Notes |
|---------|--------|-------|
| v1.0 | **Current** | Initial release. 10 pillars. CI-oriented scoring. |

Breaking changes to pillar definitions, scoring weights, or question sets constitute a major version bump. Additive changes such as new questions or clarifications are minor versions.

---

## Contributing

AWAF improves through real-world use. Contributions are welcome from anyone building or operating AI agents in production.

**In scope for this repo:**
- New questions that surface real agent failure modes
- Evidence that existing questions are too broad or too narrow
- Proposed Tier 2 pillars for failure modes not currently covered
- Case studies of agents scored against AWAF

**Not in scope for this repo:**
- Tooling changes (open issues in [awaf-cli](https://github.com/YogirajA/awaf-cli))
- Runtime-specific or framework-specific extensions (open a Discussion first)

See [CONTRIBUTING.md](CONTRIBUTING.md) for the RFC process for proposing changes. The bar for changing the specification is higher than for tooling. Changes should reflect demonstrated failure modes from production systems, not hypothetical risks.

---

## Why This Exists

The microservices era taught expensive lessons. Blast radius, cascading failures, observability debt. The patterns that prevent those failures are now well understood and encoded in every major cloud provider's well-architected framework.

Agent systems are teaching the same lessons faster, because agents take actions, hallucinate, and fail in ways that traditional infrastructure never did. A specification that encodes those lessons before teams pay for them in production incidents is worth building.

Build the slice first. Then build it well.

AWAF is that specification. It belongs to the community that builds agents.

---

## License

Apache 2.0. See [LICENSE](LICENSE).

The AWAF name and logo are trademarks of Yogiraj Aradhye. Use of the specification is unrestricted under Apache 2.0. Use of the AWAF name to imply endorsement or certification requires written permission.

---

## Related

- [awaf-cli](https://github.com/YogirajA/awaf-cli): Reference implementation with CLI, GitHub Action, and score history
- [FRAMEWORK.md](FRAMEWORK.md): Complete pillar definitions and scoring questions
- [IMPLEMENTATIONS.md](IMPLEMENTATIONS.md): Community implementations of the specification
- [Are We Building AI Agents Like We Built Microservices?](https://aradhye.com/ai-agents-well-architected-framework/): The post that introduced AWAF
