# Thermal-Fluid Research Workflow (vendored plugin)

This folder is a copy of the skills and commands from
[hanhuark/mechanical-engineering-research-skill](https://github.com/hanhuark/mechanical-engineering-research-skill)
by Han Hu, taken from upstream `main` at commit `7b6c32d` (upstream plugin
version 0.4.0). Licensed under the [MIT License](LICENSE) © 2026 Han Hu.
Everything below `skills/` and `commands/` is unchanged from upstream except for
the patches listed under [Deviations from upstream](#deviations-from-upstream).

## What it does

Research skills for thermal-fluid mechanical engineering. They are written to
keep claims tied to evidence: a correlation is checked against its validity
range, a CFD result is checked for mesh, wall treatment and validation before
it is treated as evidence, and a revision loop pauses for the author when more
data or judgment is needed.

| Skill | Use it for |
| --- | --- |
| `mechanical-engineering-research` | Coordinator for cross-cutting work: analysis, code, writing, releases, research identity. Ships 4 stdlib audit scripts (LaTeX project, technical writing, data manifest, style calibration). |
| `thermal-fluid-analysis` | Heat transfer, fluid mechanics, phase change, HVAC, energy systems, correlations, CFD, uncertainty. |
| `research-writing-literature` | Literature reviews, manuscript sections, citations, figure discussions. |
| `research-proposal-development` | Solicitation-aligned proposals, preliminary results, milestones, risks. |
| `research-data-analysis` | Baseline cases, hypothesis-driven DOE, traceable data processing, plots. |
| `research-slide-design` | Research talks, posters, speaker notes. |
| `research-schematic-design` | Editable facility, mechanism, workflow and graphical-abstract schematics. |
| `research-mentor-review` | Constructive, prioritized feedback on student work. |
| `reviewer-author-loop` | Reviewer critiques, author revises, verifier checks, repeat until accepted or paused. |

18 commands wrap these skills, for example `/me-cfd-review`,
`/me-correlation-check`, `/me-experiment-plan`, `/me-lit-review`,
`/me-proposal` and `/reviewer-author-loop`.

## Install

```bash
/plugin marketplace add alirezarezvani/claude-skills
/plugin install thermal-fluid-research-workflow@claude-code-skills
```

Codex, Gemini CLI, Hermes and Vibe users get the skills through this repo's
generated mirrors (`.codex/`, `.gemini/`, `.hermes/`, `.vibe/`). To track
upstream directly, install from the upstream repo instead.

## Try it

```text
Use the thermal-fluid-analysis skill to check whether the Dittus-Boelter correlation is valid for my laminar microchannel at Re = 800.
/me-cfd-review Review this CFD setup for mesh, y+, boundary-condition and validation gaps.
python3 skills/mechanical-engineering-research/scripts/audit_latex_project.py path/to/paper --json
```

## Deviations from upstream

This numbered list is the authoritative record. `.claude-plugin/authoring-notes.json`
summarizes it. Re-apply every item when re-vendoring.

1. **Manifests replaced.** Upstream's `.claude-plugin/plugin.json` and
   `.codex-plugin/plugin.json` are not copied. This plugin has its own
   `plugin.json` in this repo's format (`"skills": "./skills"`, repo version),
   and attribution lives in `authoring-notes.json`, this README and `LICENSE`.
2. **Not vendored:** the upstream repo-level README and its translations,
   `docs/`, `examples/`, `tests/`, `scripts/validate_repo.py` and
   `assets/branding/`. Only `assets/icon.svg` and `assets/workflow.mmd` are kept.
3. **`disable-model-invocation: true` added to all 18 commands.** Each command
   only wraps a skill that Claude can already load by itself, and without the
   flag the plugin went over this repo's skill-listing cap for a new plugin
   (gate G12: 6,030 chars against a 6,000 cap). Users run the commands exactly as before.
4. **`--sample` added to `audit_latex_project.py` and `audit_technical_writing.py`.**
   Both print JSON, and gate G9 requires such tools to carry an embedded demo
   input. The change is additive: a `SAMPLE_PROJECT` / `SAMPLE_TEXT` constant
   and a `--sample` flag. `audit_technical_writing.py`'s `source` argument
   became optional so `--sample` can run without it; omitting both still
   exits with an error. No audit logic changed.

## Updating

```bash
git clone https://github.com/hanhuark/mechanical-engineering-research-skill /tmp/thermal-fluid-upstream
cp -r /tmp/thermal-fluid-upstream/skills /tmp/thermal-fluid-upstream/commands research/thermal-fluid-research-workflow/
cp /tmp/thermal-fluid-upstream/assets/icon.svg /tmp/thermal-fluid-upstream/assets/workflow.mmd research/thermal-fluid-research-workflow/assets/
cp /tmp/thermal-fluid-upstream/LICENSE research/thermal-fluid-research-workflow/LICENSE
```

Then re-apply the deviations above, update the commit in this README and in
`authoring-notes.json`, and bump the plugin version.
