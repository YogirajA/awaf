# AWAF Implementations

Tools that implement the [AWAF specification](FRAMEWORK.md). To add yours, open a PR using the template at the bottom of this page.

An AWAF-compatible implementation should:

- Score all 10 pillars across the 3 tiers.
- Apply the tier weights (Tier 2 pillars count 1.5x; Tier 0 and Tier 1 count 1.0x).
- Enforce the Foundation gate: if Foundation scores below 40, do not score the other pillars and report a fail.
- Report readiness using the standard bands (Production Ready / Near Ready / Needs Work / High Risk / Not Ready) at the 85 / 70 / 50 / 25 / 0 boundaries.
- Display a confidence level (`verified`, `partial`, `self_reported`) alongside every score.
- State which AWAF version it targets.

---

## Reference implementations

Maintained by the AWAF authors.

### awaf-cli

- **Repo:** https://github.com/YogirajA/awaf-cli
- **Install:** `pip install awaf`
- **AWAF version:** v1.4
- **Type:** Command-line tool and CI check (Python)
- **What it does:** Runs all 10 pillars against a repository through an LLM provider of your choice (Anthropic, OpenAI, Azure OpenAI, Google, or LiteLLM). Produces a scored report with cited `file:line` findings, a self-contained HTML report, and a code-graph evidence view. Stores score history in SQLite for regression detection, and gates CI by exit code.

### awaf-skill

- **Repo:** https://github.com/YogirajA/awaf-skill
- **Install:** Claude Code plugin via the `awaf-marketplace` (`/plugin`)
- **AWAF version:** v1.4
- **Type:** Claude Code skill / plugin
- **What it does:** Runs an interactive AWAF assessment inside Claude Code, from code, cloud configs, eval reports, or a verbal description of the system. Produces the same scored report and HTML output as the CLI, with `file:line` findings.

---

## Community implementations

None yet. Yours could be the first. Open a PR adding a section below using this template:

```markdown
### <name>

- **Repo:** <url>
- **Install:** <how to install or use it>
- **AWAF version:** <v1.4 | ...>
- **Type:** <CLI | library | service | IDE plugin | ...>
- **What it does:** <one or two sentences: what it scores, from what evidence, and how it reports.>
```

Implementations are listed as a convenience to the community. Listing does not imply certification or endorsement by the AWAF authors. See [CONTRIBUTING.md](CONTRIBUTING.md) for how spec changes are proposed, and the [LICENSE](LICENSE) for use of the AWAF name.
