# Spec Delta: `security-guardrails`

## Purpose

Defines deterministic input sanitization, pre-execution injection blocking, self-contained 16-PII masking, and the 100-scenario evaluation corpus.

## ADDED Requirements

### Requirement: Length and Character Whitelist Sanitizer
The system SHALL reject input longer than `300` characters and SHALL strip characters outside `ALLOWED_CHARS`. The sanitizer MUST NOT call Google Model Armor or read `GOOGLE_CLOUD_PROJECT`.

#### Scenario: Over-long input is rejected
- **WHEN** `sanitize_input` receives a string longer than `300` characters
- **THEN** it raises `ValueError`

#### Scenario: No GCP credentials required
- **WHEN** `GOOGLE_CLOUD_PROJECT` is unset
- **THEN** `sanitize_input("Where is order 3?")` still returns a sanitized string

### Requirement: Blocker Evaluates Raw and Preprocessed Queries
`SecurityBlocker.evaluate_query` SHALL assign `processed = self._preprocess_query(query)` and SHALL match patterns against both the raw query and the URL-decoded / leetspeak-normalized form. Word-boundary patterns MUST NOT block the English word `"confirm"`.

#### Scenario: URL-encoded injection is blocked
- **WHEN** the query contains URL-encoded SQL or command injection
- **THEN** status is `BLOCKED`

#### Scenario: Confirm is allowed
- **WHEN** the query is `"Yes, please confirm"`
- **THEN** status is `PASS`

### Requirement: Self-Contained 16-PII Masker
`mask_sensitive_data(text)` SHALL redact all 16 `PII_INFO_TYPES` with asterisks using character-safe `re.sub`, including when `GOOGLE_CLOUD_PROJECT` is unset, and MUST NOT slice Python strings by UTF-8 byte offsets.

#### Scenario: Email and street address are masked without GCP
- **WHEN** `MASK` processing runs on text containing an email and a street address
- **THEN** those spans are replaced with asterisks and surrounding multibyte punctuation (`—`, `…`) is intact

### Requirement: One-Hundred Scenario Eval Corpus
The eval suite SHALL contain `61` malicious and `39` legitimate scenarios and SHALL pass `100/100` against sanitizer + blocker.

#### Scenario: Full corpus is green
- **WHEN** `pytest tests/evals/test_security_eval.py` runs
- **THEN** `100` scenarios pass and `0` fail
