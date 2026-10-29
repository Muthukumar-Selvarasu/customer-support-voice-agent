# Design: Security Guardrails & 100-Scenario Evals

## Context

See `proposal.md`. Independent of Groups 4–7. Copies `backup/cs_agent/security/*` and `backup/cs_agent/evaluation/*`. Closes PoC bugs #1 (discarded preprocess), #3 (DLP no-op), and #4 (byte-offset UTF-8 corruption), and drops Model Armor from the sanitizer.

## Goals / Non-Goals

**Goals:**
- Deterministic, GCP-free sanitizer / blocker / masker.
- `100/100` on the existing eval corpus.

**Non-Goals:**
- Hosting Judge/Masker as A2A services.
- Changing the 100 scenario texts unless a regex word-boundary tweak is required for a legitimate PASS.

## Decisions

### Decision 1: Drop Model Armor from sanitizer
Remove `google.auth`, `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`, and `MODEL_ARMOR_TEMPLATE_ID`. Keep length cap + whitelist + final regex.

### Decision 2: Use preprocess output
In `evaluate_query`, replace the discarded call with `processed = self._preprocess_query(query)` and search patterns on both `query` and `processed`.

### Decision 3: Self-contained masker
Replace `google.cloud.dlp_v2` with regexes covering the 16 `PII_INFO_TYPES`. Signature becomes `mask_sensitive_data(text: str) -> str` (no `project_id`). Use character spans, not UTF-8 byte slices.

### Decision 4: Evals live under `tests/evals/`
Move `evaluator.py`, `test_scenarios.json`, and `test_config.json` out of application code. Add `tests/evals/test_security_eval.py` as the pytest entry.

## Risks / Trade-offs

- **[Risk] Regex PII is weaker than Cloud DLP** → Mitigation: this is the agreed Railway / zero-GCP path; coverage is the 16 types in `PII_INFO_TYPES` plus unit tests for each category.
- **[Risk] Preprocess + word-boundary tweaks could fail a legitimate scenario** → Mitigation: run the full 100 and adjust `\b` patterns only, do not rewrite the corpus.
