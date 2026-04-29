# Euler Legal

Public source of truth for Euler legal documents.

## Active documents

The active production documents must always use stable filenames:

- `termsOfUse.md`
- `privacyPolicy.md`
- `riskDisclosures.md`

## Updating documents

Before replacing an active document, move the previous version into `archive/`
with a date suffix:

- `termsOfUse.md` -> `archive/termsOfUse-YYYYMMDD.md`
- `privacyPolicy.md` -> `archive/privacyPolicy-YYYYMMDD.md`
- `riskDisclosures.md` -> `archive/riskDisclosures-YYYYMMDD.md`

Then replace the stable active file with the new version.

The latest active document should never live only in a date-suffixed file.
Date-suffixed files are archives of previous versions.

## Proposed production URLs

- `/termsOfUse`
- `/privacyPolicy`
- `/riskDisclosures`

## Deployment

Production and app configuration should use a legal base URL plus the stable
paths above. When legal documents are updated, the URL paths should remain
unchanged. If the deployment host changes, only the base URL should need to
change.

Suggested configuration shape:

- `LEGAL_BASE_URL=https://legal.euler.finance`
- `LEGAL_TERMS_PATH=/termsOfUse`
- `LEGAL_PRIVACY_PATH=/privacyPolicy`
- `LEGAL_RISK_DISCLOSURES_PATH=/riskDisclosures`
