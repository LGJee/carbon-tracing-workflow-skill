# Stage handoffs

Inspect actual signatures/headers before adapting a workflow. The tested project
uses a version-3 generated plan with exactly `version`, `models`, `output_dir` at
the top level. Paths inside it are relative to the project root. Keep the same
plan across mapping, review delivery and propagation.

## Input preparation or registration

For a research question or supplied model, delegate to
`carbon-tracing-model-setup-skill`. It supplies a readable case script, one
concrete validated FBA/pFBA solution per condition, workbooks and a generated plan.
Do not impose publication conditions on new analyses. The skill handles model
selection and scientific clarification; do not repeat its completed questions.

For existing input Excel, inspect the tables before inferring a stage from a
filename. Required columns are:

| Table | Required columns |
|---|---|
| Reactions | ReactionID, ReactionName, Flux, Equation |
| Metabolites | MetaboliteID, MetaboliteName |

Fluxes are signed, finite and nonzero. Metabolite/source names include matching
compartments. Missing SMILES is allowed at this stage; step2 handles existing
annotations, companion files, manual structures and database completion.

Use the project's input readers and plan helpers rather than writing another
exporter. Its `examples/step1_use_existing_inputs.py` may target the default
publication plan: do not run it unchanged for a new case. Adapt a registration
script inside the case folder, resolve the actual project root and save only
that case's plan. Compute hashes from actual files. Preserve original inputs.
Ask for ordered sources/compartments only if trustworthy supplied records cannot
establish them. Do not derive source identity or uptake solely from a filename.

For mapping-only inputs, inspect `Mapping_Info`, original input provenance,
structures and the plan. The mapping workbook's own hash is different from its
`SourceInputSHA256`, which identifies the original step-1 input. Never replace one
with the other to satisfy a check. Missing trusted plan/source settings requires
recovering those records or asking for the missing data, not inventing them.

The tested exporter keeps `pFBA` in input filenames even for FBA and rejects
nonpositive biological objectives. Record the actual method and disclose a
blocked valid request rather than modifying checks or fabricating objective values.

## Mapping

From the actual project root:

```bash
python3 step2_atom_mapping.py --plan results/results_by_AI/my_analysis/analysis_plan.json
```

Use LocalMapper, RXNMapper and EnzymeMap for the default full workflow. Retain
the project's direction, stoichiometry, pure-transport, unmappable-input and
consensus rules. Let its CPU/memory capacity limit independent mapper jobs;
do not add an outer parallel pool that oversubscribes the same computer.
Wait for stage completion before reviewing final selections. Reuse compatible
manual_smiles.csv structures and manual_atom_mappings.csv carbon records first.
Step 2 may use a known-structure subreaction while preserving the full model
reaction; saved omissions require optional scientific review, not fabricated
protein structures. Pass the original equation and omission context to the review
skill and do not treat a reduced input as complete original-reaction coverage.

The stage may reuse predictions after its input/settings checks. A cache hit is
not established by the workbook name alone. Preserve raw predictions, scores,
`Mapping_Selection`, `Final_Mapping`, structures and provenance. A successfully
executed tool can still return failed reactions; report these separately.

## Default scientific review

For a complete analysis invoke carbon-tracing-review-skill. Read its installed
SKILL.md and applicable handoff references. Delegate its full review workflow:
check files, complete justified missing structures, remap affected ready reactions
under project Rules, rebuild all review targets, invoke enzymatic-atom-mapping,
validate decisions, deliver copies/reports and persist evidence. Do not implement
another workbook editor, structure resolver or reaction-review pipeline here.
The review skill owns scientific delegation even when all mappers fail. Explicitly
request review-only when delegating: this orchestrator owns subsequent step3 and
runs it once after review delivery. Standalone review otherwise runs step 3 by
default. Record that handoff to prevent duplicate propagation.

Pass the actual workbooks, project location and selected plan's output folder
explicitly. Preserve SourceInputSHA256: it identifies the original step1 input,
not a staging workbook's own hash. Structure completion may update structures
and affected mapper/selection/final rows in a separate staging copy. Subsequent
correspondence review changes Final_Mapping only relative to that staged input.
Keep originals, unrelated rows and previous completed deliveries.

Review targets include TRUE flags, blank mappings/missing final rows and newly
affected reactions, subject to the review skill's Boundary/NoCarbon exemptions.
Pass saved partial-correspondence records from hidden Mapping_Info and omission context; do not require a Partial_Mapping sheet. Reduced correspondences remain Partial;
scientifically rejected/unresolved partials are disabled in the AI copy under the
review skill's validator. New AI proposals retain that skill's coverage rules.
AIReviewed records actual scientific analysis; AIGenerated records accepted
replacements. Preserve ReviewRequired by default. None certifies correctness.

Manual mapping records use Species, ReactionID, MappedReactionSMILES and retain
priority. AI discoveries belong in ai_smiles.csv and
ai_atom_mappings.csv with Source, plus immutable structure/mapping archives.
Never turn AI discoveries into manual overrides implicitly. Reuse candidates
only through exact-context checks and validators. Structure completion alone
does not set reaction-level AIGenerated or AIReviewed. Let the review skill
manage these records; the orchestrator links the resulting evidence and reports.

No targets means a reported no-op. Reviewed retained/unresolved decisions may
produce a review-only copy with no new AIGenerated flags. Do not manufacture an
AI workbook by renaming an untouched file. Reconcile report counts and flags;
record unassessed targets, blocked operations and archive/delivery errors.

Put legitimate <model>_2_atom_mapping_AIGenerated.xlsx copies in this plan's
resolved output folder using the review helper's explicit --output-dir. Its
fallback can select the publication plan instead of the custom case. Preserve
same-named prior deliveries and resolve versioning deliberately. An output
elsewhere is not automatically discovered by step3: report or resolve the handoff
using compatible existing interfaces. Never move/delete other inputs to manipulate
mapping-version discovery.

## Unified carbon analysis

```bash
python3 step3_carbon_flow.py --plan results/results_by_AI/my_analysis/analysis_plan.json
```

The current step3 runs fixed-flux single-carbon origin balances (size-1
EMU-inspired) only. Inspect the actual installed code/docs; an older combined
checkout has a different contract and is not automatically certified by this
update. Do not invent a --method option, restore enumeration, add a step4 or use
rounds. The default command above runs all available ordinary/AI mappings for the
selected planned models. Each version is independent.

Outputs remain <model>_3_carbon_flow.xlsx and
<model>_3_carbon_flow_AIGenerated.xlsx. Each new file contains only
EMU_Metabolites and EMU_BoundaryInputs. The metabolite sheet omits FluxResidual,
Status, StructureType, KnownCarbonCount and ContributionScope, which the solver
keeps internally. Source/Unknown columns and original flux turnover are unchanged.
A blank CarbonCount on a partial structure means its full carbon count is unknown;
its mean source fractions cover explicit known positions only. Polymer totals
can be unavailable. Do not turn missing values into zero or infer a complete
molecular contribution.

Skipping new review does not exclude existing AI copies from discovery. For an
explicit ordinary-only request, use supported mapping_files in the internal
run_carbon_flow API, or an isolated, correctly registered input case after checking
the actual interface. --output-dir changes only the result destination, not which
mapping versions are found. Do not hide/delete AI files or alter trusted hashes.

Settings belong to step3, not the generated plan: recycling currently defaults
True through EMU_ALLOW_INORGANIC_CARBON_RECYCLING and tolerance to 1e-8 through
EMU_TOLERANCE. Record actual values and authorized overrides. Inorganic-carbon
recycling does not establish biological CO2 loss. Keep external imports distinct
and retain Unknown; do not redistribute or renormalize known source fractions.

The shared scheduler limits independent model/version jobs by CPU/live memory,
and no enumeration chunk/state workers remain. Keep the existing mapper and
solver environments, memory/disk checks and atomic size-limited save helper.
A complete oversized EMU candidate is rejected, returning nonzero and preserving
any previous workbook; there are no saved rounds or resumable round checkpoints.

Validate exit status, current carbon_flow_run.json and input/mapping hashes.
For each mapping_file, emu_solved means a numerical solve finished;
emu_completed=true with output_file and stop_reason=emu_solved means the complete
workbook was published. Distinguish memory_limit, disk_space_limit,
file_size_limit, error and not_completed. The emu object records numerical checks,
assumptions and mapping_version. On batch failure another worker may have saved a
complete file; report its actual state without calling the whole batch successful.
An early validation failure can leave an old report; verify invocation context.
No Summary, Carbon_Flow or carbon_flow_summary.xlsx is generated in the current
version. Preserve historical files and do not use their presence as completion.

## Shared AI work directory

Create disposable workflow work under `<project>/.cache/ai/workflow/<run>/`.
Use it for redirected terminal logs, downloaded research pages, intermediate
JSON, proposals and staged copies; pass the chosen folder to the review skill.
The review helper's default caches also live under .cache/ai/, and its structure
completion helper defaults to .cache/ai/structures/. Do not override these with
an output/results root just to save intermediate files. Final AI workbooks go
to the plan's output directory; per-model final reports go to its Ai_reports/ folder.
`report_files` is a per-input list with input_file, markdown and cached json paths.
Clearing work caches does not clear ai_smiles.csv, ai_atom_mappings.csv, original
models, analysis plans or the durable scientific review archives in data/.
