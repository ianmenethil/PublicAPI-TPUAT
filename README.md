# TravelPay APIs - UAT

This repository automatically downloads the Swagger 2.0 JSON specification from
`https://apiuat.travelpay.com.au/v2.0/help` weekly, adds a missing Basic Auth security scheme, converts it to **OpenAPI 3.1**, and splits the OpenAPI spec into multiple files for easier consumption by tools, humans, and automated agents.

---

## Files

* `swagger.json` — Original downloaded Swagger 2.0 specification (Basic Auth injected post-download).
* `openapi.json` — Converted **OpenAPI 3.1** single-file specification (authoritative for consumers).
* `openapi/` — Split OpenAPI 3.1 files (paths, schemas, security, etc.).
* `.well-known/llm.json` — Machine-readable manifest that tells automated agents and LLMs where the authoritative artifacts live and how to retrieve them. Read this first if you are building tooling or an agent that needs deterministic access.

---

## Automation

The repository contains a GitHub Actions workflow (`Weekly API Update`) that:

1. Downloads `swagger.json` from the upstream endpoint.
2. Adds `BasicAuth` and performs JSON post-processing with `jq`.
3. Converts `swagger.json` to `openapi.json` (OpenAPI 3.1) using the Scalar CLI.
4. Splits the OpenAPI spec into `openapi/`.
5. Commits and pushes changes **only if** content actually changed.

Because the workflow only commits when the API contract changes, the canonical “latest” raw URL is stable and points to the most recently validated spec.

---

## Manual Trigger

To run the update manually (for testing), open **Actions → Weekly API Update**, select the workflow and click **Run workflow**.

---

## Tools Used

* [Scalar CLI](https://github.com/scalar/scalar) — OpenAPI conversion and splitting.
* `jq` — JSON post-processing (add BasicAuth, normalization).
* GitHub Actions — scheduling and execution.

---

## Machine-Readable / LLM Support

This repository exposes a manifest at the predictable raw URL:

```
https://raw.githubusercontent.com/ianmenethil/PublicAPI-TPUAT/main/.well-known/llm.json
```

**How automated agents / LLMs should behave**

1. **Fetch the manifest** from the raw URL on `defaultBranch` (the manifest contains `defaultBranch`, `rawBaseUrl`, `files`, and `instructions`).
2. **Use the manifest** to build canonical raw URLs (e.g., `rawBaseUrl + '/' + defaultBranch + '/' + files.openapiSpec`) to fetch `openapi.json`.
3. **Do not** scrape HTML pages on GitHub. Use raw content URLs (or the GitHub API).
4. **Prefer `openapi.json`** as the authoritative spec. Use `openapi/` only when a tool requires the split files.
5. Follow any `instructions` and `security` hints contained in the manifest (the manifest includes hints such as `security.authSchemes`).

---

## Canonical retrieval (always-latest)

For normal, “get the latest” consumption, use these stable raw URLs on the canonical branch (`main`):

```text
https://raw.githubusercontent.com/ianmenethil/PublicAPI-TPUAT/main/openapi.json
https://raw.githubusercontent.com/ianmenethil/PublicAPI-TPUAT/main/swagger.json
https://raw.githubusercontent.com/ianmenethil/PublicAPI-TPUAT/main/openapi/
https://raw.githubusercontent.com/ianmenethil/PublicAPI-TPUAT/main/.well-known/llm.json
```

These URLs are stable (the URL does not change). The content at those URLs changes **only** when the workflow commits a new spec.

---

## Security / Authentication

* The OpenAPI spec documents two common security schemes used by this API: `Api-Key` (API key in header) and `BasicAuth` (HTTP Basic authentication).
* Many operations list both schemes in their `security` requirement (i.e., both `Api-Key` and `BasicAuth` are expected). Always inspect the operation’s `security` value in the spec.
* Do not commit secrets or API keys into the repository. Use environment variables, secret stores, or CI secrets for runtime authentication.
