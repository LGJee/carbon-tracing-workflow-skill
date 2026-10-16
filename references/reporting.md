# Concise workflow report

Write `workflow_report.md` in the analysis directory (or the user's accessible
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
- Deliverable paths, validation evidence, errors, unfinished work and next action.

Review outcome partitions and review flags overlap: do not add AIReviewed to the
sum of replaced/retained/unresolved/unassessed. Unchanged or rejected maps do not
imply unreviewed, and AI-generated does not mean correct. Missing final rows can
make saved flag totals differ from initial review-target counts; explain this.

Compare coverage at stated, shared calculated rounds when possible. If versions
stopped at different rounds, report their individual ranges; do not invent later
observations or automatic fits. Results describe possible source-label reachability
under selected network/mapping assumptions, not measured isotope abundance,
quantitative carbon allocation, physical feeding time or experimental confirmation.

An old workbook or aggregate is not proof of current success. An error can occur
before a new run report exists. File-size stops return normally but are not
convergence; memory stops retain earlier rounds but are not resumable checkpoints.
Review failures can leave valid independent outputs; list both rather than
calling the whole workflow successful or discarding valid work.
