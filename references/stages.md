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
Wait for stage completion before reviewing final selections.

The stage may reuse predictions after its input/settings checks. A cache hit is
not established by the workbook name alone. Preserve raw predictions, scores,
`Mapping_Selection`, `Final_Mapping`, structures and provenance. A successfully
executed tool can still return failed reactions; report these separately.

## Default scientific review

For a complete analysis invoke `carbon-tracing-ai-review-skill`; it delegates
reaction evidence and correspondence analysis to `enzymatic-atom-mapping`.
Delegate workbook review/delivery only; this orchestrator owns subsequent merged
step3 propagation, so the review skill must not also run its legacy step4 route.
Use its existing inspect/apply helpers and JSON schema. Do not write another
workbook editor or duplicate the scientific analysis. It targets the union of
TRUE review flags, including failed reactions absent from Final_Mapping.

Keep raw predictions and Mapping_Selection unchanged. Accepted replacements
belong only in the delivered copy's Final_Mapping. `AIReviewed` means scientific
analysis occurred; `AIGenerated` means an accepted replacement. `ReviewRequired`
is preserved by default. None of these flags certifies experimental correctness.

The helper archives records under `data/ai_mapping_reviews/`. Reuse compatible
candidate evidence through its validator, not by automatically accepting a cache.
Rejected or incompatible records are not accepted mappings. Distinguish record
counts from unique chemical transformations and current-run changes from old flags.

No flagged rows is a reported no-op, with no artificial AI copy. No accepted
replacement still requires the actual review report. The review helper can
deliver review-only copies when scientific retained/unresolved decisions exist;
their AIGenerated flags remain unchanged. Follow its behavior rather than renaming
an untouched workbook to manufacture success. Explain any archive/delivery error.

Put legitimate `<model>_2_atom_mapping_AIGenerated.xlsx` copies in this plan's
output folder. Pass that folder explicitly with the review helper's
`--output-dir`; its fallback may select the publication plan instead of this
case. Preserve prior same-named reviewed deliverables; resolve a new
version deliberately instead of overwriting them silently. An output delivered
elsewhere is not automatically discovered by step3. Record that integration gap
or arrange a compatible analysis plan/output location using existing interfaces.
Do not remove/move other inputs merely to influence automatic discovery.

## Unified carbon analysis

```bash
python3 step3_carbon_flow.py --plan results/results_by_AI/my_analysis/analysis_plan.json
```

Step 3 runs Enumeration only; `--output-dir` isolates results without changing
which step-2 mappings the plan selects. Original and `_AIGenerated` copies may
exist alone or together. Available versions run independently and produce
`_3_carbon_flow_Enumeration.xlsx` and
`_3_carbon_flow_Enumeration_AIGenerated.xlsx`. Input hashes, mapping direction,
structures and mapping provenance remain validated. No method flag is needed.
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
current complete rounds, not reuse of an older output. Temporary candidates may
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
