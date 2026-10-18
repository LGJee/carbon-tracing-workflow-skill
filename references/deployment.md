# Deployment and dependency routing

Use [the project](https://github.com/LGJee/atom-mapped-carbon-tracing) and its
actual installed documentation. Respect an explicitly selected checkout; do not
infer compatibility from its folder name or origin. Record its commit and whether
local changes are present. A base commit alone does not identify a modified tree.

## Environment and project

Find the user's accessible Linux host/WSL distribution rather than assuming one.
The tested installation specifications target Linux/WSL x86_64. Report missing
access or unsupported platforms accurately. A user-managed remote server needs
an available, authorized connection; do not claim deployment from a plan alone.

For deployment, locate Git, Conda, the project and data paths. If the project is
missing, obtain its official source in a new user-appropriate directory. Install
missing task dependencies using existing installers; never delete another
environment or reset an existing checkout. If an existing installation is
incompatible, identify the failed check and propose a specific repair or separate
compatible install. Installing the project does not imply OS replacement/reboots.

The current project provides:

```bash
bash scripts/setup_environments.sh
python3 scripts/download_enzymemap.py
```

The installer checks/reuses `.envs/preparation`, `.envs/localmapper`,
`.envs/rxnmapper` and `.envs/enzymemap`. Preparation uses COBRApy/GLPK; LocalMapper's
environment also drives tracing. Separate mapper processes use their own Python.
For a custom environment prefix, inspect the project's documented preparation,
mapping and tracing interpreter settings; do not ask the user to switch shells
or environments for every stage. Do not silently upgrade pinned dependencies.

Use existing `scripts/check_environment.py` checks with each relevant interpreter.
Preparation performs a small actual pFBA solve, LocalMapper/RXNMapper checks call
their interfaces, and EnzymeMap's dependency check executes a rule. The latter
does not alone establish that the downloaded EnzymeMap dataset matches a supplied
reaction. Check the dataset/rules configuration and trusted-file hashes; use a
small full adapter example when certifying end-to-end mapping.

Install only the capabilities required by the requested stages. Full project
deployment includes the four environments; propagation of existing mappings does
not require retraining/downloading mapper models. Successful imports or file
existence alone do not establish that a tool works. Use small checks and report
which actual calls ran. Do not start the six reference models for deployment-only.

## Skill dependencies

Use available installed skills first. When a needed skill is absent, use the
available `skill-installer`, reading its current instructions before invoking
it. Install official repository roots under the correct local names:

| Local name | Repository |
|---|---|
| carbon-tracing-model-setup-skill | https://github.com/LGJee/carbon-tracing-model-setup-skill |
| carbon-tracing-review-skill | https://github.com/LGJee/carbon-tracing-review-skill |
| enzymatic-atom-mapping | https://github.com/LGJee/enzymatic-atom-mapping-skill |

Do not bundle their contents or treat skill names as shell executables. Read and
apply their instructions using the host's available capabilities. Model setup is
needed only when preparing/modifying models; AI review needs the review and
scientific skills. A complete deployment should make all three available.
Record source/revision when available. Do not overwrite existing skill versions
or modify them just to conceal an interface discrepancy.

## Known compatibility differences

Some older project copies have MATLAB preparation or a separate step4, while
the tested Python development version has merged ordinary/AI tracing in step3.
Inspect step3's actual method settings, documentation and output naming. The
current development interface supports Enumeration and size-1 EMU-inspired source
balances through ANALYSIS_METHODS; older checkouts may support Enumeration alone.
Report capabilities and use supported methods without silently changing settings.
Current outputs omit the legacy _Enumeration suffix and combine selected methods
in one workbook per mapping version. Follow the installed entrypoint; do not
recreate step4 or silently alter dependencies. Older versions lacking the required
plan/export interface are not certified by this skill.
