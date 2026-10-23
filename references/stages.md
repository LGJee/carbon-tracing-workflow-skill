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
Pass Partial_Mapping and omission context. Reduced correspondences remain Partial;
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

Newer step 3 selects Enumeration, size-1 EMU-inspired source balances, or both
through ANALYSIS_METHODS at the script top. Inspect actual settings; do not assume
both are available in an older checkout or change the user's method selection.
Both methods read the same mapping
version and share one result workbook; inspect actual installed capabilities.
EMU is not a full isotopomer simulator. Enumeration exports R1-R3 details in one Carbon_Flow sheet, with Carbon_Flow_Round first,
by default while later rounds continue updating Summary. EMU_Metabolites and
EMU_BoundaryInputs belong to the independent balance solver, with separate EMU
recycling/tolerance settings. No separate method workbooks are created; `--output-dir` isolates results without changing
which step-2 mappings the plan selects. Original and `_AIGenerated` copies may
exist alone or together. Available versions run independently and produce
`_3_carbon_flow.xlsx` and
`_3_carbon_flow_AIGenerated.xlsx`. Input hashes, mapping direction,
structures and mapping provenance remain validated. Use the installed script's
settings; do not invent a --method option or require the legacy _Enumeration suffix.
Review-only copies may therefore produce identical carbon coverage. Explain this
rather than inventing an AI improvement or rerunning a separate step4.

Skipping new AI review does not exclude existing AI copies from discovery. Explain
this distinction. For an explicit ordinary-only request, inspect supported version
selection controls first; if unavailable, explain the interface gap and propose
an isolated plan/output location for the selected copies. Preserve originals;
do not hide or delete AI files to manipulate discovery.

For Enumeration, use existing round/resource settings and authorized `--rounds` overrides. Do not
reinvoke mappers. Default inorganic-carbon recycling is disabled: inorganic
endpoints are retained but their labels do not feed later reactions; other
substrates still propagate. This is an analysis assumption, not evidence that
CO2 has negligible biological importance. Record the actual selected setting.

For Enumeration, the current default is 50,000,000 bytes with `OUTPUT_BATCH_ROWS = 50000`.
Use disk-staged output and actual candidate-XLSX checks at batch/layer boundaries.
On size overflow, stop enumeration, discard the unfinished round and keep the
preceding complete workbook and Summary. No pagination is emitted: a round that
cannot fit one sheet stops with `row_limit`. Only complete rounds below the
size threshold atomically replace the output. First-round rejection means zero
current complete Enumeration rounds and preserves the previous combined output.
EMU-only runs can save independently. emu_solved means calculated;
emu_completed means successfully delivered. Temporary candidates may
overshoot between checks. Memory guards remain necessary for accumulated states;
staged rows are not resumable checkpoints.

For Enumeration, use process exit status and the current `carbon_flow_run.json`, not filenames
alone, to establish completed rounds and outputs. Ordinary/AI records can share
a model name; identify the mapping file and its hash. Earlier validation failure
can leave an old report, so verify it belongs to the invocation. Step 3 also rebuilds `carbon_flow_summary.xlsx` from existing Enumeration
workbooks in the output directory (one Summary per sheet, including AI versions).
It does not search nested cases or recalculate data. Older saved files can be
included; only the run manifest establishes current-run status. Long sheet names
are shortened, `_AI` marks reviewed mappings, and numeric suffixes resolve collisions.

## Shared AI work directory

Create disposable workflow work under `<project>/.cache/ai/workflow/<run>/`.
Use it for redirected terminal logs, downloaded research pages, intermediate
JSON, proposals and staged copies; pass the chosen folder to the review skill.
The review helper's default caches also live under .cache/ai/, and its structure
completion helper defaults to .cache/ai/structures/. Do not override these with
an output/results root just to save intermediate files. Final AI workbooks go
to the plan's output directory; per-model final reports go to its reports/ folder.
`report_files` is a per-input list with input_file, markdown and cached json paths.
Clearing work caches does not clear ai_smiles.csv, ai_atom_mappings.csv, original
models, analysis plans or the durable scientific review archives in data/.
