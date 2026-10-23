# Concise workflow report

Write `Ai_reports/workflow_report.md` in the analysis directory (or the user's accessible
delivery location for deployment-only). Link existing preparation/review reports
and Excel files. Do not create another statistics workbook or decorative sheets.

Record:

- Purpose, organism/model sources, culture/comparison conditions and assumptions.
- Project path/revision plus local-change status, relevant interpreter versions,
  available dependency skills, actual commands and execution settings.
- Stage outcomes: completed, skipped/not needed, partial, failed or not started.
  Deployment-only must explicitly show that scientific analysis was not started.
- Per workbook: mapped/failed original reactions, flagged targets, scientifically
  reviewed records, accepted current replacements, retained/unresolved/unassessed
  targets, and pre-existing AI provenance separately. Reconcile counts against
  the review helper's saved JSON/report; no review report means unknown, not zero.
- Ordinary/AI mapping paths/hashes, actual saved rounds, observed coverage and
  stop reasons from current results. Include the Summary denominator definition
  from the installed implementation (currently carbon-containing metabolite IDs
  participating in usable final mappings, not all metabolites in the full model).
- Review-stage structure discoveries and affected remapping outcomes from its
  detailed report; link AI CSV indexes and archives without recomputing their
  counts or promoting them to manual data. Mapper failure is distinct from
  scientific review failure.
- Actual selected methods, method-specific recycling settings, and their saved
  tables. Do not require Summary for an EMU-only output or compare mean fractions
  directly with Enumeration coverage.
- Deliverable paths, validation evidence, errors, unfinished work and next action.

Review outcome partitions and review flags overlap: do not add AIReviewed to the
sum of replaced/retained/unresolved/unassessed. Unchanged or rejected maps do not
imply unreviewed, and AI-generated does not mean correct. Missing final rows can
make saved flag totals differ from initial review-target counts; explain this.

Compare coverage at stated, shared calculated rounds when possible. If versions
stopped at different rounds, report their individual ranges; do not invent later
observations or automatic fits. Enumeration describes possible source-label
reachability, not quantitative carbon-source fractions, measured isotope
abundance, physical feeding time or experimental confirmation.
On projects supporting EMU, report the selected size-1 source-balance method,
source/Unknown fractions, mapping version, recycling assumption, and numerical
validation separately from Enumeration coverage. These are model-dependent
mean origin fractions, not a full isotopomer simulation or a measured allocation.
Distinguish emu_solved from emu_completed when combined delivery is interrupted.

An old workbook is not proof of current success. An error can occur
before a new run report exists. File-size stops return normally but are not
convergence; memory stops retain earlier rounds but are not resumable checkpoints.
Review failures can leave valid independent outputs; list both rather than
calling the whole workflow successful or discarding valid work.

## File placement

Put raw logs, extraction JSON, candidates, retry reports and orchestration state
in the selected project's `.cache/ai/workflow/<run>/`; pass that work folder to
child skills. Review's final Markdown belongs in the case's Ai_reports/ folder, one
per model, combining its SMILES and mapping changes. Its detailed process JSON
stays in .cache/ai/review/. Keep prepared scripts/plans and adopted chemistry and
evidence outside disposable cache. Describe clearing the cache as discarding
intermediate work, not deleting final results. Group conversation output by model.
