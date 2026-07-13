# Contributing to AWAF

AWAF is an open specification. It improves through real-world use, and contributions are welcome from anyone building or operating AI agents in production. The bar for changing the specification is deliberately high: changes should reflect demonstrated failure modes from production systems, not hypothetical risks.

## What belongs here

**In scope for this repo:**

- New questions that surface real agent failure modes.
- Evidence that an existing question is too broad or too narrow.
- Proposed Tier 2 pillars for failure modes not currently covered.
- Case studies of agents scored against AWAF.
- Corrections and clarifications to [FRAMEWORK.md](FRAMEWORK.md).

**Not in scope for this repo:**

- Tooling changes. Open those in [awaf-cli](https://github.com/YogirajA/awaf-cli/issues).
- Runtime-specific or framework-specific extensions. Open a Discussion first.

To list a tool that implements AWAF, see [IMPLEMENTATIONS.md](IMPLEMENTATIONS.md); that does not go through the RFC process.

## The RFC process

Specification changes go through a lightweight RFC (Request for Comments) so the reasoning is recorded alongside the change.

1. **Raise it.** Open a GitHub Issue (or Discussion) describing the failure mode you want the spec to catch or the problem with an existing question. Include evidence: a real incident, a class of bug, or a pattern you have seen fail in production. Proposals without a demonstrated failure mode will be asked for one.
2. **Propose it.** If there is appetite, open a PR that edits `FRAMEWORK.md`. The PR description is the RFC and must state:
   - **The change:** the exact question, criterion, or pillar being added, removed, or reworded.
   - **The rationale:** the demonstrated failure mode it addresses.
   - **The version impact:** major or minor (see Versioning below), with a one-line justification.
   - **Migration notes:** what an existing adopter or implementation must change, if anything.
3. **Review.** A maintainer and the community discuss on the PR. Expect pushback: the question is always "has this failure mode actually been observed in production?"
4. **Land it.** On acceptance, the same PR (or a follow-up) updates the version table in [README.md](README.md), bumps the version banner in `FRAMEWORK.md`, and the release is tagged. Implementations then adopt the new version at their own pace and update their listing in `IMPLEMENTATIONS.md`.

## Versioning

AWAF is versioned independently of any tool that implements it.

- **Major version** (for example v1 to v2): breaking changes to pillar definitions, scoring weights, tier assignments, or question sets. Existing scores are not comparable across a major bump.
- **Minor version** (for example v1.3 to v1.4): additive changes such as new questions, new advisory signals, or clarifications that do not change how existing answers score.

Every change to the specification content must move the version. Prose fixes that do not change meaning (typos, formatting) do not.

## Style

- Follow the writing style in [CLAUDE.md](CLAUDE.md): no em dashes in prose sentences.
- Keep questions concrete and answerable from evidence. A good question can be answered "yes / partially / no" by looking at code, configuration, or operational artifacts.

## License

By contributing you agree that your contribution is licensed under Apache 2.0, the same license as the specification. See [LICENSE](LICENSE).
