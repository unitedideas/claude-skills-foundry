# Contributing to claude-skills-foundry

Thanks for wanting to contribute a Claude Code skill. Skills here should be practical, focused, and useful to agents running real workloads — not demos.

## Filing an issue

- Search existing issues first.
- For bugs in an existing skill: open a **Bug report** with the form.
- For new skill ideas: open a **Skill request** describing what workflow the skill automates and what trigger phrases should invoke it.

## Suggesting a skill

A good skill is scoped to one workflow, triggers on clear phrases, and documents its inputs/outputs. Skills should compose with other skills rather than duplicate them.

## Submitting a pull request

1. Fork and branch from `main`.
2. Add your skill under `skills/<short-name>/` with a `SKILL.md` matching the format of existing skills (description, trigger conditions, steps).
3. Every new skill should score **85 or higher** on the [NHS agent-readiness rubric](https://nothumansearch.ai/score) for its documentation surface — agents should be able to discover and reason about the skill from its metadata alone.
4. Keep skills portable; avoid hard-coded local paths.
5. Open the PR using the pull request template; fill in "what", "why", and "test plan".

## Code of conduct

Be kind, be specific, be useful. That's it.
