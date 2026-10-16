# Carbon Tracing Workflow Skill

A Codex skill that deploys and orchestrates
[atom-mapped-carbon-tracing](https://github.com/LGJee/atom-mapped-carbon-tracing).
Start from a research question, metabolic model, input Excel workbook or existing
atom mappings. The skill selects the necessary stages, reuses existing tools,
checks their outputs and writes a concise English `workflow_report.md`.

## Install and use

Ask Codex to install this repository with its skill installer:

```text
Use $skill-installer to install https://github.com/LGJee/carbon-tracing-workflow-skill
```

Invoke the installed skill with your project location and request, for example:

```text
Use $carbon-tracing-workflow-skill to deploy the carbon-tracing project and check
its installation. Do not start an analysis yet.
```

```text
Use $carbon-tracing-workflow-skill to compare E. coli supplied with glucose alone
versus simultaneous glucose and xylose. Help me establish the model and conditions.
```

```text
Use $carbon-tracing-workflow-skill to continue my existing analysis from these
mapping workbooks and their analysis plan.
```

The skill can communicate in the user's language. Generated scripts, instructions
and reports use English. Compatible existing installations are reused; missing
dependencies are installed as needed without silently replacing existing ones.

## Workflow and dependent skills

Deployment checks → input preparation → three-tool mapping and selection →
AI-assisted review → carbon propagation → verification and delivery.

Only the requested stages run. Deployment alone does not launch the publication
models, and suitable existing inputs avoid unnecessary upstream calculations.

| Responsibility | Existing capability |
|---|---|
| Select/configure models and export FBA/pFBA inputs | [carbon-tracing-model-setup-skill](https://github.com/LGJee/carbon-tracing-model-setup-skill) |
| Review flagged mappings and deliver reviewed copies | [carbon-tracing-ai-review-skill](https://github.com/LGJee/carbon-tracing-ai-review-skill) |
| Establish evidence-supported enzymatic correspondences | [enzymatic-atom-mapping-skill](https://github.com/LGJee/enzymatic-atom-mapping-skill), installed as `enzymatic-atom-mapping` |
| Predict and propagate mappings | Existing project scripts: LocalMapper, RXNMapper, EnzymeMap and carbon propagation |

AI review is enabled by default for a complete analysis unless excluded by the
user. No review targets means a reported no-op, not an artificial AI copy.
Accepted replacements affect only `Final_Mapping` in a reviewed copy; raw
predictions and `Mapping_Selection` remain unchanged. The fixed computation
scripts do not call an LLM themselves.

## Compatibility and interpretation

This skill targets the Python/COBRApy project interface with generated analysis
plans and a merged `step3_carbon_flow.py` that discovers ordinary and
`_AIGenerated` mappings. It does not require a separate step4. Some dependent
documentation still mentions step4; inspect the actual project before running.
Older checkouts lacking this interface are not certified by the validation below.
An existing newer local development checkout must not be replaced with an older
public revision automatically. This skill release does not publish or upgrade
the main computation project.

The tested platform is Linux under WSL on x86_64. Native Windows, macOS and ARM
execution and a fresh installation were not verified. Inspect current project
settings rather than assuming platform support or silently upgrading dependencies.

The tested default size stop saves a complete round, then stops that result if
its workbook reaches 50,000,000 bytes. The retained file may exceed this threshold.
This is separate from memory protection. Propagation reruns start at round 1.

Coverage describes possible carbon-origin label propagation and reachability
under the chosen network, constraints and mappings. It is not experimental isotope
abundance, physical cultivation time or quantitative carbon allocation.

## Validation

Validated against the current local Python development project using the official
skill validator, independent routing evaluations and a small real three-mapper
execution followed by one propagation round. The structured example contained
two reactions and no review targets; no new scientific AI review was claimed.
Separate synthetic checks covered review bookkeeping, ordinary/AI propagation,
provenance checks, resource guards and file-size stops. Full publication models
and cross-platform deployment were not rerun for this skill release.

See [SKILL.md](SKILL.md) for the agent workflow and its linked references for
deployment, stage handoffs and reporting conventions. Project code, models,
environments, caches and private results are not bundled in this repository.
