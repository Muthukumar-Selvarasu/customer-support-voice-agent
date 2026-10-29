# Tasks: `03-security-guardrails-and-evals`

## 1. Sanitizer and blocker

- [ ] 1.1 Copy `backup/cs_agent/security/sanitizer.py` to `src/security/sanitizer.py`, remove Model Armor / `google.auth` / `GOOGLE_CLOUD_*`, keep `MAX_INPUT_LENGTH = 300` and `ALLOWED_CHARS`, and verify with `pytest tests/unit/test_security.py`
- [ ] 1.2 Copy `backup/cs_agent/security/blocker.py` to `src/security/blocker.py`, assign `processed = self._preprocess_query(query)` and evaluate patterns on raw + processed text, and verify `"Yes, please confirm"` PASSes with `pytest tests/unit/test_security.py`

## 2. Masker and eval corpus

- [ ] 2.1 Copy `backup/cs_agent/security/masker.py` to `src/security/masker.py`, replace Cloud DLP with character-safe `re.sub` across all 16 `PII_INFO_TYPES` (no `project_id`), and verify with `pytest tests/unit/test_security.py`
- [ ] 2.2 Copy `backup/cs_agent/evaluation/{evaluator.py,test_scenarios.json,test_config.json}` to `tests/evals/`, add `tests/evals/test_security_eval.py`, and verify `100/100` with `pytest tests/unit/test_security.py tests/evals/test_security_eval.py`
