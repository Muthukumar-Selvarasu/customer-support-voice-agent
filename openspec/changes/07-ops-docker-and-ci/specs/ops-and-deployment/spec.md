# Spec Delta: `ops-and-deployment`

## Purpose

Defines local orchestration, containerization, CI, and operator docs for the multi-service cascade stack.

## ADDED Requirements

### Requirement: Multi-Process Local Orchestration
`run.sh` SHALL be valid bash and SHALL be able to start Postgres, MCP Toolbox, A2A Judge, A2A Masker, optional Phoenix, and the web/voice app as separate processes using `src/` entrypoints.

#### Scenario: Script parses
- **WHEN** `bash -n run.sh` runs
- **THEN** it exits `0`

### Requirement: Generated Config Has No GCP Lock-In
`run.sh` generated `.env` and `tools.yaml` MUST NOT contain `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`, or `MODEL_ARMOR_TEMPLATE_ID`, and `get-order-status` MUST use `WHERE order_id = $1 AND customer_email = $2`.

#### Scenario: Heredoc is GCP-free and tenant-scoped
- **WHEN** `run.sh` config templates are inspected
- **THEN** those three GCP keys are absent and `get-order-status` includes `customer_email`

### Requirement: Container Image and CI
`Dockerfile` SHALL install Python 3.12+ dependencies from `requirements.txt` and default to the web/voice entrypoint. `.github/workflows/ci.yml` SHALL run `pytest` for `tests/unit`, `tests/integration`, and `tests/evals`.

#### Scenario: CI workflow exists
- **WHEN** `.github/workflows/ci.yml` is loaded
- **THEN** it contains a `pytest` step covering unit, integration, and evals

### Requirement: README Matches the Repo
`README.md` SHALL document `./run.sh`, required `GOOGLE_API_KEY` / `MEM0_API_KEY` / `DATABASE_URL`, Railway multi-service notes, and MUST NOT instruct operators to set `GOOGLE_CLOUD_PROJECT`.

#### Scenario: README has no GCP project requirement
- **WHEN** `README.md` is read
- **THEN** setup steps list `GOOGLE_API_KEY` and do not require `GOOGLE_CLOUD_PROJECT`
