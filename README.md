# Mono (mono-africa)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Mono is an open banking and financial-data infrastructure company for Africa - often described as the "Plaid for Africa" - headquartered in Lagos, Nigeria and acquired by Flutterwave in January 2026. Its REST APIs let businesses link customer bank accounts and read financial data (accounts, transactions, identity, income, balance, statements, assets, earnings), assess creditworthiness, collect direct bank payments via DirectPay (direct debit), and verify identity data (BVN, NIN, CAC, account numbers) through the Lookup services.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/mono-africa/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/mono-africa/refs/heads/main/apis.yml)

## Access Model (Honest Summary)

- **Self-serve dashboard keys.** Sign up on the Mono dashboard, create an application, and get **test** and **live** key pairs. Each app has a **public key** (used only by the front-end Connect / DirectPay widgets) and a **secret key** (used for all server-to-server calls).
- **Authentication:** server-to-server requests send the secret key in the **`mono-sec-key`** header. There is no OAuth client-credentials flow for the server API; the end-user's bank consent happens inside the hosted **Mono Connect** widget, which returns an authorization code you exchange (`POST /v2/accounts/auth`) for a permanent **account id**.
- **Test vs live:** build for free against the **sandbox** with test keys; going **live** requires business/KYC onboarding and is billed on usage.
- **Base host:** `https://api.withmono.com` - most endpoints under `/v2`; the account-number and Mashup **Lookup** endpoints are under `/v3`.
- **No public WebSocket.** The API is REST over HTTPS plus outbound **webhooks** (account/data-status and DirectPay payment events). No `wss://` streaming API is documented.

> Fidelity note: endpoint paths and methods below are grounded in the public docs at [docs.mono.co](https://docs.mono.co). Mono does not publish a machine-readable OpenAPI/Postman file that was located, so the request/response **schemas** in `openapi/` are representative models - confirm exact payloads against the live reference. Modeled items are flagged in `review.yml`.

## Tags

- Open Banking
- Financial Data
- Payments
- Fintech
- Account Linking
- Direct Debit
- Bank Data
- Africa
- Nigeria
- Financial Infrastructure

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Mono Connect API

Account-linking authorization. Initiate a linking session for the hosted Mono Connect widget, then exchange the returned authorization code for a permanent account id used on all subsequent financial-data calls.

- **Human URL:** [https://docs.mono.co/docs/financial-data/overview](https://docs.mono.co/docs/financial-data/overview)
- **Base URL:** `https://api.withmono.com/v2`

### Mono Financial Data API

Read financial data from a linked account by account id - account details, transaction history, identity/KYC, real-time balance, income analysis, and bank statements (JSON or PDF) - plus unlink an account when consent ends.

- **Human URL:** [https://docs.mono.co/api/bank-data/transactions](https://docs.mono.co/api/bank-data/transactions)
- **Base URL:** `https://api.withmono.com/v2`

### Mono Investment Data API

Retrieve investment holdings for a linked account - assets and earnings/returns - for wealth and portfolio use cases.

- **Human URL:** [https://docs.mono.co/api/bank-data/investment/assets](https://docs.mono.co/api/bank-data/investment/assets)
- **Base URL:** `https://api.withmono.com/v2`

### Mono Creditworthiness API

Assess a customer's repayment capacity from their linked-account financial history, returning an affordability/creditworthiness analysis to support lending and credit-decisioning workflows.

- **Human URL:** [https://docs.mono.co/api/bank-data/accounts/credit-worthiness](https://docs.mono.co/api/bank-data/accounts/credit-worthiness)
- **Base URL:** `https://api.withmono.com/v2`

### Mono DirectPay API

Collect direct bank payments without cards. Initiate a one-time debit payment (paid via the DirectPay widget), verify a payment's status by reference, and list all payments. Give value only after the verify endpoint confirms a successful payment.

- **Human URL:** [https://docs.mono.co/api/directpay/initiate](https://docs.mono.co/api/directpay/initiate)
- **Base URL:** `https://api.withmono.com/v2`

### Mono Lookup API

Verify and validate user- and company-submitted data for Nigerian KYC/KYB - BVN (consent-based), account-number ownership, and the Mashup identity lookup. Account-number and Mashup lookups are served under `/v3/lookup`; BVN uses `/v2/lookup`.

- **Human URL:** [https://docs.mono.co/docs/lookup/overview](https://docs.mono.co/docs/lookup/overview)
- **Base URL:** `https://api.withmono.com`

## Common Properties

- [Domain Security](security/mono-africa-domain-security.yml)
- [Authentication](authentication/mono-africa-authentication.yml)
- [GitHub Organization](https://github.com/withmono)
- [LinkedIn](https://www.linkedin.com/company/withmono)
- [Website](https://mono.co/)
- [Documentation](https://docs.mono.co)
- [Plans](plans/mono-africa-plans-pricing.yml)
- [Rate Limits](rate-limits/mono-africa-rate-limits.yml)
- [Fin Ops](finops/mono-africa-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
