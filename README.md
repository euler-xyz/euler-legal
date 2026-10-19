# Euler Legal

Public source of truth for Euler legal documents.

## Active documents and public URLs

The active documents on `main` use stable filenames. The marketing site renders
them at these public URLs:

| Active document | Public page |
| --- | --- |
| [termsOfUse.md](termsOfUse.md) | [Terms of Use](https://www.euler.finance/terms) |
| [privacyPolicy.md](privacyPolicy.md) | [Privacy Policy](https://www.euler.finance/privacy-policy) |
| [riskDisclosures.md](riskDisclosures.md) | [Risk Disclosures](https://www.euler.finance/risk-disclosures) |

Keep both the filenames and public URL paths stable when updating the content.

## Updating documents

1. Create a branch from the latest `main` with the proposed legal wording and
   effective date ready for review.
2. Before replacing an active document, preserve its previous contents in
   `archive/` under a date-suffixed filename. Keep existing archives unchanged.
3. Put the replacement document at the same stable filename. Check the effective
   date wherever it appears, including at the end of the document.
4. Open a pull request targeting `main`. Review the legal wording, dates, links,
   rendered Markdown, and archived copy before merging.
5. Merge to `main`, then complete the downstream verification below. Consumers
   that follow `main` can pick up the update as soon as it is merged.

Archive filename patterns:

- `termsOfUse.md` -> `archive/termsOfUse-YYYYMMDD.md`
- `privacyPolicy.md` -> `archive/privacyPolicy-YYYYMMDD.md`
- `riskDisclosures.md` -> `archive/riskDisclosures-YYYYMMDD.md`

The latest active document should never live only in a date-suffixed file.
Date-suffixed files are archives of previous versions.

## Downstream updates

| Consumer | How it reads the documents | Action after a content update |
| --- | --- | --- |
| [Marketing site](https://github.com/euler-xyz/euler-marketing-site/blob/main/src/content/LegalPage.tsx) | Resolves the latest `main` commit and fetches the selected document at that revision when rendering a legal page. The pages are dynamic and the fetches use `no-store`. | No rebuild required. Verify the public pages above. |
| [Euler Lite](https://github.com/euler-xyz/euler-lite/blob/master/docs/tos-signing.md) | `/api/internal/tos` fetches the Markdown URL configured by `NUXT_PUBLIC_CONFIG_TOS_MD_URL`. Privacy and risk links point to the marketing site by default. | No rebuild required for document content changes **if the configured URL follows the active document on `main`**. Allow caches to refresh, then reload the app. |
| [Euler Toolbox](https://github.com/euler-xyz/euler-toolbox/blob/main/docs/toolbox-onboarding-terms.md) | `/api/internal/legal-documents` fetches all three active documents from `main`. | No rebuild required. Allow caches to refresh, then reload the app. |

For Lite, check the deployed configuration: an empty `NUXT_PUBLIC_CONFIG_TOS_MD_URL`
disables Terms signing, and a URL pinned to a commit or archive does not follow
updates to `main`. Configuration or application-code changes follow that app's
deployment process.

### Cache timing and acceptance

- **Lite:** the server and browser cache Terms for five minutes. The browser
  response permits ten minutes of stale-while-revalidate; edge headers permit a
  one-hour cache and a one-day stale-while-revalidate window. An open app also
  retains its fetched Terms in memory until a reload. See the
  [cache settings](https://github.com/euler-xyz/euler-lite/blob/master/nuxt.config.ts)
  and [client loader](https://github.com/euler-xyz/euler-lite/blob/master/composables/useTosData.ts).
- **Toolbox:** the server keeps a complete set fresh for five minutes and can use
  it for ten more minutes if the upstream refresh fails. Fresh responses permit
  a one-hour shared cache and a one-day stale-while-revalidate window; actual edge
  behavior depends on the hosting platform. Documents are checked on app load.
- **Acceptance:** Lite derives its signed message from the Terms content and the
  configured Terms page URL. Changes to either require acceptance of the new
  version once loaded; privacy or risk changes alone do not change that hash.
  Toolbox hashes all three documents along with its acknowledgement copy and
  links, so a change to any document requires browser acceptance again after the
  new content loads. Formatting changes also affect these hashes.

Automatic propagation is not an immediate rollout guarantee. Upstream and edge
caches can delay visibility, and reloading the app does not clear those caches.
Verify the content actually served before treating an update as live.

## Verify a published update

1. Check that the active raw Markdown below contains the merged wording and date.
2. Open each affected public page and check its wording, effective date, links,
   and formatting.
3. Check the target Lite deployment's `/api/internal/tos` response, when Terms
   signing is enabled, and Toolbox's `/api/internal/legal-documents` response
   against the active documents. Reload each app after the updated content is
   served and check that the expected acknowledgement prompt appears where
   applicable.

Raw GitHub URLs for Markdown consumers:

- [Terms of Use](https://raw.githubusercontent.com/euler-xyz/euler-legal/refs/heads/main/termsOfUse.md)
- [Privacy Policy](https://raw.githubusercontent.com/euler-xyz/euler-legal/refs/heads/main/privacyPolicy.md)
- [Risk Disclosures](https://raw.githubusercontent.com/euler-xyz/euler-legal/refs/heads/main/riskDisclosures.md)
