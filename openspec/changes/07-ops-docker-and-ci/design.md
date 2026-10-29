# Design: Ops, Docker & CI

## Context

See `proposal.md`. Depends on Groups 1–6. Copies `backup/run.sh` and `backup/README.md`. Adds `Dockerfile` and `.github/workflows/ci.yml` (not in backup).

## Goals / Non-Goals

**Goals:**
- One local command to boot the 5–6 services separately.
- CI that reruns every group's pytest.
- Railway-ready image without GCP env vars.

**Non-Goals:**
- Provisioning a Railway project in this slice.
- Shipping secrets in git.

## Decisions

### Decision 1: Keep run.sh as the local orchestrator
Update paths from `cs_agent.*` to `src.api.web`, `src.a2a.run_servers`, and `src.api.cli`. Keep Postgres/Toolbox/A2A/Phoenix/web as separate processes.

### Decision 2: gen_config_files stays the source of generated YAML
`tools.yaml` in Group 1 is the committed contract. `run.sh` heredoc must match it (tenant-scoped `get-order-status`, no GCP keys) so `./run.sh setup` cannot overwrite the fix.

### Decision 3: Single Dockerfile, many Railway services
One image, different start commands per Railway service (`web`, `a2a-judge`, `a2a-masker`, `mcp-toolbox`, `postgres` plugin, optional `phoenix`). Private hostnames `*.railway.internal`.

### Decision 4: CI runs the full suite
Unit tests always run. Integration/evals may skip live Gemini/Postgres via pytest markers, but the workflow still invokes those directories.

## Risks / Trade-offs

- **[Risk] run.sh setup overwrites tools.yaml** → Mitigation: keep heredoc in lockstep with Group 1 SQL; add a CI grep that the generated statement includes `customer_email`.
