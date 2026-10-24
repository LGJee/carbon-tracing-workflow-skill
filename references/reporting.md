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
- Ordinary/AI mapping paths and hashes, output files, emu_solved,
  emu_completed and stop_reason from this invocation's manifest. Verify current
  report context and output sheets; no round counters or Summary are required.
- Review-stage structure discoveries and affected remapping outcomes from its
  detailed report; link AI CSV indexes and archives without recomputing their
  counts or promoting them to manual data. Mapper failure is distinct from
  scientific review failure.
- EMU source/Unknown fractions, external-source categories, recycling setting,
  tolerance and numerical checks. On partial structures report the known-position
  scope even though internal scope columns are omitted from the exported sheet.
- Deliverable paths, validation evidence, errors, unfinished work and next action.

Review outcome partitions and review flags overlap: do not add AIReviewed to the
sum of replaced/retained/unresolved/unassessed. Unchanged or rejected maps do not
imply unreviewed, and AI-generated does not mean correct. Missing final rows can
make saved flag totals differ from initial review-target counts; explain this.

Compare ordinary and reviewed source fractions only for compatible conditions,
flux solution, sources and solver settings, disclosing any mapping/structure
changes. These are model-dependent mean origins, not measurements or full
isotopomer simulations. A larger resolved fraction is not proof of higher
accuracy. Keep Unknown visible and do not allocate it among known sources.

An old workbook is not proof of current success. Errors can occur before a new
report exists. Size, memory and disk failures return nonzero and keep previous
files unchanged; emu_solved may be true while emu_completed is false. Review
failure and downstream calculation failure are different outcomes. List valid
independent deliverables and unfinished work without claiming batch success.

## File placement

Put raw logs, extraction JSON, candidates, retry reports and orchestration state
in the selected project's `.cache/ai/workflow/<run>/`; pass that work folder to
child skills. Review's final Markdown belongs in the case's Ai_reports/ folder, one
per model, combining its SMILES and mapping changes. Its detailed process JSON
stays in .cache/ai/review/. Keep prepared scripts/plans and adopted chemistry and
evidence outside disposable cache. Describe clearing the cache as discarding
intermediate work, not deleting final results. Group conversation output by model.
