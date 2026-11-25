# PrivacyTern — proposed AWS architecture

PrivacyTern is built on AWS services designed for sensitive data: detection with Comprehend, context with Bedrock, and encryption anchored in KMS — all in your chosen region.

## Request flow

1. Support records
2. AWS KMS
3. AWS Lambda
4. Comprehend
5. Bedrock
6. Privacy review
7. Redacted export

## Services

### Amazon Comprehend

**Role:** Detect personal information

Identifies names, addresses, phone numbers, emails, payment references and other personal data in support text, with a confidence score for each entity.

### Amazon Bedrock

**Role:** Understand sensitive context

Detects sensitive personal context that pattern matching misses, and classifies contact reasons for aggregate analytics — with Guardrails to keep outputs safe.

### AWS KMS

**Role:** Encrypt everything

Encrypts records at rest and protects reversible redaction tokens with customer-managed keys, so only authorised roles can ever reveal an original value.

### AWS Lambda

**Role:** Run detection and redaction

Processes each record in isolation: detect, apply policy, route to review, and write results — without storing raw text longer than necessary.

### Amazon DynamoDB

**Role:** Store findings and audit logs

Holds detection findings, review decisions, policy versions and the audit trail of every reveal and export.

## Principles

- **Minimise first** — Redact what isn’t needed before anyone sees it.
- **Customer-held keys** — Reversible tokens use keys your organisation controls.
- **Aggregates only** — Analytics never expose an individual conversation.

## Safeguards

- **Synthetic demos** — Demonstrations and early pilots use synthetic records only — never real customer data.
- **Reviewer approval** — Exports require a privacy reviewer’s sign-off.
- **Purpose-bound reveal** — Seeing a redacted value needs permission and a logged reason.
- **Encrypted end to end** — KMS encryption protects data at rest and every token.

## MVP scope

We generate synthetic support records that mirror your real ticket types, configure your redaction policies, and run the full detect–redact–review–export workflow with your privacy team before any real data is involved.

- Synthetic record generation
- Redaction policy setup
- Detection and redaction
- Privacy review queue
- Reviewer-approved exports
