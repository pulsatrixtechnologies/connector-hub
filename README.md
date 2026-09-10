<p align="center">
  <img src="logo.png" width="72" alt="Pulsatrix">
</p>

<h1 align="center">Pulsatrix connector hub</h1>

<p align="center">
  Product index for the Pulsatrix connector.<br>
  Official vendor MCP, OpenAPI, Swagger, GraphQL and RPC.
</p>

<p align="center">
  <a href="index.json"><img src="https://img.shields.io/badge/format-pulsatrix--hub%2F1-3c76f4?style=flat-square&labelColor=111a2f" alt="format pulsatrix-hub/1"></a>
  <a href="index.json"><img src="https://img.shields.io/badge/products-557-3c76f4?style=flat-square&labelColor=111a2f" alt="557 products"></a>
  <a href="#remote-mcp"><img src="https://img.shields.io/badge/MCP-164%20remote-3c76f4?style=flat-square&labelColor=111a2f" alt="164 remote MCP"></a>
  <a href="https://github.com/pulsatrixtechnologies/connector-hub/commits/main"><img src="https://img.shields.io/github/last-commit/pulsatrixtechnologies/connector-hub?style=flat-square&labelColor=111a2f&color=3c76f4" alt="last commit"></a>
  <a href="https://github.com/pulsatrixtechnologies/connector-hub/issues"><img src="https://img.shields.io/github/issues/pulsatrixtechnologies/connector-hub?style=flat-square&labelColor=111a2f&color=3c76f4" alt="issues"></a>
  <a href="https://pulsatrix.ca"><img src="https://img.shields.io/badge/website-pulsatrix.ca-111a2f?style=flat-square" alt="pulsatrix.ca"></a>
</p>

<p align="center">
  <a href="https://pulsatrix.ca">Website</a>
  ·
  <a href="https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json">Raw index</a>
  ·
  <a href="https://github.com/pulsatrixtechnologies/connector-hub/issues/new?template=add-product.md">Add a product</a>
  ·
  <a href="mailto:hello@pulsatrix.ca">Contact</a>
</p>

One file, [`index.json`](index.json), format `pulsatrix-hub/1`. Each entry is a vendor an MSP actually runs: PSA, RMM, EDR, identity, cloud, billing, and the SaaS around them. When that vendor hosts an MCP server, the URL is on the card. When they publish OpenAPI, Swagger, GraphQL or RPC, the catalog `source` is too.

Nothing is preloaded. Adding a product from the console imports the catalog. If the vendor already hosts MCP, connect that in the AI client first and skip the compile unless you need Pulsatrix profiles, journal and scopes.

```
https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json
```

557 products in 20 categories. 164 with a vendor-hosted MCP URL, 4 with official MCP docs only (local, self-hosted, or per-account). 112 carry a GraphQL schema source and 44 are an MCP server the connector imports as a catalog. The gateway refuses a `hub_url` larger than 256 KiB; the file is written compact and stays under that.

## Contents

1. [Use it](#use-it)
2. [What this repository is](#what-this-repository-is)
3. [Format](#format)
4. [MCP or catalog](#mcp-or-catalog)
5. [Tags](#tags)
6. [Categories](#categories)
7. [What belongs here](#what-belongs-here)
8. [Remote MCP](#remote-mcp)
9. [Fetchable specs](#fetchable-specs)
10. [GraphQL endpoints](#graphql-endpoints)
11. [Login-gated documents](#login-gated-documents)
12. [Checked](#checked)
13. [Limits](#limits)
14. [Not listed](#not-listed)
15. [Contribute](#contribute)
16. [Status](#status)
17. [License](#license)

## Use it

**From the console.** Products starts empty. Search, filter by tag, open a card. If the product has `mcp.url`, that is the primary action. Add catalog is the fallback. Profiles then grant the product to people.

**From a connector.** The gateway ships a bundled copy of this index. To pin a fork or a revision:

```
hub_url = "https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json"
```

HTTPS only. The response must be `pulsatrix-hub/1` and at most 256 KiB.

**From a script.**

```bash
curl -sL https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json \
  | jq '.products[] | select(.mcp.url) | {name, url: .mcp.url}'
```

```bash
jq '.products[] | select(.tags | index("account")) | .name' index.json
```

```bash
jq '.products[] | select(.id == "pagerduty")' index.json
```

The compiled catalogs live in the connector repo (`catalog/`). This hub is the index, not a second copy of every document.

## What this repository is

| Path | Role |
|---|---|
| [`index.json`](index.json) | The registry. Name, tags, catalogs, `source`, optional `mcp`. |
| [`.github/ISSUE_TEMPLATE/add-product.md`](.github/ISSUE_TEMPLATE/add-product.md) | How to ask for a vendor that is missing. |

Unknown JSON fields are ignored. `installed` is filled at list time from catalogs already in the store. Do not put it in the file.

## Format

`format` must be `pulsatrix-hub/1`.

```json
{
  "format": "pulsatrix-hub/1",
  "name": "Pulsatrix connector hub",
  "community": {
    "repo": "https://github.com/pulsatrixtechnologies/connector-hub",
    "request_url": "https://github.com/pulsatrixtechnologies/connector-hub/issues/new?template=add-product.md"
  },
  "products": []
}
```

### Product

| Field | Required | Notes |
|---|---|---|
| `id` | yes | Stable slug. Unique. Snake case. |
| `name` | yes | Card title. |
| `vendor` | yes | Company that ships it. |
| `summary` | yes | One or two sentences. What an MSP uses it for. |
| `mark` | yes | Exactly two characters, unique across the file. Shown on the card. |
| `tone` | yes | Console accent. Common values: `query`, `write`, `user`, `macro`, `error`. |
| `tags` | yes | See [Tags](#tags). `mcp` first when `mcp` is present. |
| `catalogs` | yes | At least one. |
| `default_catalogs` | no | Catalog ids imported on Add when the operator does not pick a subset. |
| `category` | yes | One of the twenty [categories](#categories). What the product does, not how its document is shaped. |
| `mcp` | no | Official vendor MCP. See below. |

### Catalog

| Field | Required | Notes |
|---|---|---|
| `id` | yes | Unique across the whole index. |
| `title` | yes | Short label on the card. |
| `file` | yes | Filename the connector looks up under `catalog/` (`pagerduty.json`). |
| `source` | no | HTTPS URL of the OpenAPI, Swagger, GraphQL or RPC document, of a GraphQL or MCP endpoint when `kind` says so, or of the vendor developer page when the file is login-gated. |
| `kind` | no | The import kind when `source` is not a document to download: `graphql` (the connector posts the introspection query) or `mcp_tools` (it shakes hands and reads `tools/list`). Absent means a plain document. |
| `introspection` | no | `graphql` only: `open`, `auth`, `closed` or `unknown`, as observed. `closed` means the operator opens it once on the instance, takes the schema and closes it again. |
| `endpoint_template` | no | A per-tenant endpoint with placeholders (`https://{instance}.example.com/graphql`). It is never the `source`, which stays a URL that resolves: the console fills the placeholders from what the operator types. |
| `bot_blocked` | no | The page is alive in a browser and answers 403 or 429 to any fetcher. Set so a link checker does not report it as dead every run. |
| `no_public_document` | no | The vendor publishes no API document any more. The card stays because the product is still run; `source` points at the product page. |

### MCP

| Field | Required | Notes |
|---|---|---|
| `url` | no | Remote MCP endpoint the vendor hosts. Streamable HTTP (`/mcp`) preferred over SSE (`/sse`) when both exist. |
| `docs` | no | Vendor page that documents the server. Used when the URL is local, self-hosted, or per-account. |

At least one of `url` or `docs` must be set when `mcp` is present.

PagerDuty, as it sits in the file:

```json
{
  "id": "pagerduty",
  "name": "PagerDuty",
  "vendor": "PagerDuty",
  "summary": "Incidents, on-call and services. EU MCP: https://mcp.eu.pagerduty.com/mcp",
  "mark": "PG",
  "tone": "query",
  "tags": ["mcp", "rest", "openapi"],
  "catalogs": [
    {
      "id": "pagerduty",
      "title": "REST API",
      "file": "pagerduty.json",
      "source": "https://raw.githubusercontent.com/PagerDuty/api-schema/main/reference/REST/openapiv3.json"
    }
  ],
  "mcp": {
    "url": "https://mcp.pagerduty.com/mcp",
    "docs": "https://developer.pagerduty.com/docs/mcp-tooling-remote-server"
  }
}
```

## MCP or catalog

Two different paths. Do not mix them up.

1. **Vendor MCP.** Point the AI client at `mcp.url`. Auth is the vendor's (OAuth or API key). Pulsatrix never sees the traffic.
2. **Pulsatrix catalog.** Import the OpenAPI (or Swagger, GraphQL, RPC) document. The connector compiles it, serves tools over `/mcp`, and the console can attach profiles, credentials, journal and scopes.

Use (1) when the vendor already hosts a server and you only need that product in the client. Use (2) when several people share a Pulsatrix host and you need the operator controls.

Shared endpoints:

- Atlassian Rovo MCP `https://mcp.atlassian.com/v2/mcp` covers Jira Cloud, Jira Service Management, Confluence Cloud and Bitbucket Cloud.
- PagerDuty EU is `https://mcp.eu.pagerduty.com/mcp`.
- Cloudflare publishes several MCP hosts (Workers bindings, Observability, Radar, docs) besides `https://mcp.cloudflare.com/mcp`.

## Tags

| Tag | Meaning |
|---|---|
| `mcp` | Official vendor MCP. Connect it in the client before compiling, or import it as a catalog (`kind: mcp_tools`), which absorbs its read tools into one query tool. |
| `rest` | HTTP REST. |
| `openapi` | OpenAPI document. |
| `swagger` | Swagger 2 document. |
| `graphql` | GraphQL. The catalog carries `kind: graphql`, and the connector asks the service for its own schema (`import <endpoint> --kind graphql`). |
| `rpc` | RPC methods (JSON-RPC or similar). |
| `account` | Spec is behind a login, a tenant console, or an instance page. `source` is the vendor page, not a fetchable file. |
| `private` | Private API: the vendor's own site, with the client's authorisation. |

Filter in the console or with `jq`. Combining `mcp` and `account` is normal: the MCP may be public while the OpenAPI is not.

## Categories

Every product carries exactly one `category`. The tags say how a document is shaped; the category says what the product does, which is what an operator searches by and what the console groups on.

| Category | Products |
|---|---|
| `security` | 68 |
| `msp` | 53 |
| `productivity` | 51 |
| `devtools` | 45 |
| `data` | 42 |
| `finance` | 40 |
| `cloud` | 31 |
| `identity` | 27 |
| `monitoring` | 27 |
| `crm` | 24 |
| `network` | 23 |
| `other` | 23 |
| `backup` | 22 |
| `cms` | 22 |
| `ai` | 19 |
| `ecommerce` | 14 |
| `marketing` | 10 |
| `storage` | 7 |
| `communication` | 5 |
| `hr` | 4 |

The category is also the shard key: an index that outgrows the 256 KiB cap splits into one file per category, with `index.json` keeping the cards and each shard keeping the catalogs. That split is not needed yet.

```bash
jq -r '.products[] | select(.category == "msp") | .name' index.json
```

## What belongs here

Listed:

- A vendor-hosted MCP on that vendor's domain, with a URL they publish, **or**
- An OpenAPI, Swagger, GraphQL or RPC document an operator can import, public or login-gated.

Not listed:

- Community wrappers and unofficial OpenAPI reverse-engineers. `source` stays on the vendor page.
- Aggregator MCP (one URL that proxies many apps). Zapier is listed as Zapier, not as a directory of everyone else's APIs.
- Invented MCP URLs. If the vendor did not publish it, it is not here. Per-account or local servers get `mcp.docs` and no `url`.
- Secrets, tenant hostnames, company names, keys.

Login-gated products stay in the index. The issue template already asks whether a login is required. An operator who holds the vendor document can still import it.

## Remote MCP

URLs below are the vendor hosts from `index.json`. Auth is OAuth or an API key unless the vendor says otherwise. Prefer `/mcp` over `/sse` when they publish both.

### Cloud and infrastructure

| Product | MCP |
|---|---|
| Amazon Web Services | https://aws-mcp.us-east-1.api.aws/mcp |
| AWS Knowledge | https://knowledge-mcp.global.api.aws |
| Cloudflare | https://mcp.cloudflare.com/mcp |
| Cloudflare Observability | https://observability.mcp.cloudflare.com/sse |
| Cloudflare Radar | https://radar.mcp.cloudflare.com/sse |
| Cloudflare Workers Bindings | https://bindings.mcp.cloudflare.com/sse |
| Google BigQuery | https://bigquery.googleapis.com/mcp |
| Google Compute Engine | https://compute.googleapis.com/mcp |
| Google Kubernetes Engine | https://container.googleapis.com/mcp |
| Google Maps | https://mapstools.googleapis.com/mcp |
| Microsoft Graph | https://mcp.svc.cloud.microsoft/enterprise |
| Microsoft Learn | https://learn.microsoft.com/api/mcp |
| Neon | https://mcp.neon.tech/mcp |
| Netlify | https://netlify-mcp.netlify.app/mcp |
| Prisma Postgres | https://mcp.prisma.io/mcp |
| Render | https://mcp.render.com/mcp |
| Supabase | https://mcp.supabase.com/mcp |
| Vercel | https://mcp.vercel.com/ |

### Payments, banking, spend

| Product | MCP |
|---|---|
| Mercado Libre | https://mcp.mercadolibre.com/mcp |
| Mercado Pago | https://mcp.mercadopago.com/mcp |
| PayPal | https://mcp.paypal.com/mcp |
| Pennylane | https://app.pennylane.com/mcp |
| Plaid | https://api.dashboard.plaid.com/mcp/sse |
| Ramp | https://ramp-mcp-remote.ramp.com/mcp |
| Spendesk | https://public-api.spendesk.com/v1/mcp |
| Square | https://mcp.squareup.com/sse |
| Stripe | https://mcp.stripe.com |
| Stytch | https://mcp.stytch.dev/mcp |

### CRM, support, incidents

| Product | MCP |
|---|---|
| Attio | https://mcp.attio.com/mcp |
| Close | https://mcp.close.com/mcp |
| HubSpot | https://mcp.hubspot.com |
| Intercom | https://mcp.intercom.com/sse |
| PagerDuty | https://mcp.pagerduty.com/mcp |
| Pipedrive | https://mcp.pipedrive.ai/mcp |

### Source, issues, CI

| Product | MCP |
|---|---|
| Bitbucket Cloud | https://mcp.atlassian.com/v2/mcp |
| Buildkite | https://mcp.buildkite.com/mcp |
| Confluence Cloud | https://mcp.atlassian.com/v2/mcp |
| GitHub | https://api.githubcopilot.com/mcp/ |
| GitLab | https://gitlab.com/api/v4/mcp |
| Jam | https://mcp.jam.dev/mcp |
| Jira Cloud | https://mcp.atlassian.com/v2/mcp |
| Jira Service Management | https://mcp.atlassian.com/v2/mcp |
| Linear | https://mcp.linear.app/mcp |
| Postman | https://mcp.postman.com/minimal |
| Semgrep | https://mcp.semgrep.ai/mcp |
| Sentry | https://mcp.sentry.dev/mcp |
| Stack Overflow | https://mcp.stackoverflow.com |

### Work, files, design, commerce

| Product | MCP |
|---|---|
| Airtable | https://mcp.airtable.com/mcp |
| Asana | https://mcp.asana.com/mcp |
| Ashby | https://mcp.ashbyhq.com/mcp/v1 |
| Box | https://mcp.box.com |
| Canva | https://mcp.canva.com/mcp |
| ClickUp | https://mcp.clickup.com/mcp |
| Coda | https://coda.io/apis/mcp |
| Cloudinary | https://asset-management.mcp.cloudinary.com/sse |
| draw.io | https://mcp.draw.io/mcp |
| Egnyte | https://mcp-server.egnyte.com/sse |
| Excalidraw | https://mcp.excalidraw.com |
| Figma | https://mcp.figma.com/mcp |
| Fireflies | https://api.fireflies.ai/mcp |
| Miro | https://mcp.miro.com/ |
| monday.com | https://mcp.monday.com/sse |
| Notion | https://mcp.notion.com/mcp |
| Sanity | https://mcp.sanity.io |
| Slack | https://mcp.slack.com/mcp |
| Webflow | https://mcp.webflow.com/sse |
| Wix | https://mcp.wix.com/mcp |

### Analytics, SEO, data, other

| Product | MCP |
|---|---|
| Ahrefs | https://api.ahrefs.com/mcp/mcp |
| Amplitude | https://mcp.amplitude.com/mcp |
| Apify | https://mcp.apify.com |
| Astro Docs | https://mcp.docs.astro.build/mcp |
| Clay | https://mcp.clay.earth/mcp |
| Context7 | https://mcp.context7.com/mcp |
| Cortex | https://mcp.cortex.io/mcp |
| DeepWiki | https://mcp.deepwiki.com/mcp |
| Exa | https://mcp.exa.ai/mcp |
| Globalping | https://mcp.globalping.dev/sse |
| Hex | https://app.hex.tech/mcp |
| Honeycomb | https://mcp.honeycomb.io/mcp |
| Hugging Face | https://hf.co/mcp |
| Indeed | https://mcp.indeed.com/claude/mcp |
| Malware Patrol | https://mcp.malwarepatrol.net/v1 |
| Mixpanel | https://mcp.mixpanel.com/mcp |
| OpenZeppelin | https://mcp.openzeppelin.com/contracts/solidity/mcp |
| Parallel Search | https://search-mcp.parallel.ai/mcp |
| Parallel Task | https://task-mcp.parallel.ai/mcp |
| Port | https://mcp.port.io/v1 |
| PostHog | https://mcp.posthog.com/mcp |
| Replicate | https://mcp.replicate.com/sse |
| Semrush | https://mcp.semrush.com/v1/mcp |
| SISTRIX | https://api.sistrix.com/mcp/ |
| Statista | https://api.statista.ai/v1/mcp |
| Telnyx | https://api.telnyx.com/v2/mcp |
| ThoughtSpot | https://agent.thoughtspot.app/mcp |
| Wolfram | https://agenttools.wolfram.com/mcp |
| X | https://api.x.com/mcp |
| X Docs | https://docs.x.com/mcp |
| Zapier | https://mcp.zapier.com/api/mcp/mcp |

Official MCP with no single public URL (run locally, self-host, or substitute an account id):

| Product | Docs |
|---|---|
| Databricks | https://docs.databricks.com/aws/en/generative-ai/mcp/ |
| Google Cloud | https://docs.cloud.google.com/mcp/overview |
| Grafana | https://grafana.com/docs/grafana/latest/developer-resources/mcp/ |
| Microsoft Azure | https://learn.microsoft.com/en-us/azure/developer/azure-mcp-server/overview |
| MongoDB Atlas | https://www.mongodb.com/docs/mcp-server/ |
| Snowflake | https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp |

## Fetchable specs

These `source` URLs returned the document without a vendor login when last checked. The connector still has to compile them; a few are larger than the current YAML node budget.

| Product | Catalog | Source |
|---|---|---|
| AlertOps | `alertops` | https://api.alertops.com/swagger/v1/swagger.json |
| Asana | `asana` | https://raw.githubusercontent.com/Asana/openapi/master/defs/asana_oas.yaml |
| Autotask PSA | `autotask` | https://webservices.autotask.net/atservicesrest/swagger/docs/v1 |
| Auvik | `auvik` | https://auvikapi.us1.my.auvik.com/spec |
| Axcient x360 | `axcient_*` | https://developer.axcient.com/specs/{product}.yaml |
| Bitbucket Cloud | `bitbucket` | https://api.bitbucket.org/swagger.json |
| CircleCI | `circleci` | https://circleci.com/api/v2/openapi.json |
| Cisco Meraki | `cisco_meraki_*` | https://raw.githubusercontent.com/meraki/openapi/master/openapi/spec3.json |
| Cloudflare | `cloudflare` | https://raw.githubusercontent.com/cloudflare/api-schemas/main/openapi.yaml |
| Datto RMM | `datto_rmm` | https://pinotage-api.centrastage.net/api/v3/api-docs/Datto-RMM-v2 |
| DigitalOcean | `digitalocean` | https://raw.githubusercontent.com/digitalocean/openapi/main/specification/DigitalOcean-public.v2.yaml |
| GitHub | `github` | https://raw.githubusercontent.com/github/rest-api-description/main/descriptions/api.github.com/api.github.com.yaml |
| GitLab | `gitlab` | https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/api/openapi/openapi_v2.yaml |
| Grafana | `grafana` | https://raw.githubusercontent.com/grafana/grafana/main/public/api-merged.json |
| HaloPSA | `halo_psa` | https://halo.haloservicedesk.com/api/swagger/v2/swagger.json |
| Huntress | `huntress` | https://api.huntress.io/v1/swagger_doc.json |
| Jira Cloud | `jira` | https://developer.atlassian.com/cloud/jira/platform/swagger-v3.v3.json |
| Kubernetes | `kubernetes` | https://raw.githubusercontent.com/kubernetes/kubernetes/master/api/openapi-spec/swagger.json |
| LaunchDarkly | `launchdarkly` | https://app.launchdarkly.com/api/v2/openapi.json |
| Microsoft Graph | `microsoft_graph` | https://raw.githubusercontent.com/microsoftgraph/msgraph-metadata/master/openapi/v1.0/openapi.yaml |
| N-able N-central | `n_central` | https://documentation.n-able.com/N-central/preview/rest_api_preview/Content/Resources/swaggerapi/dist/openapi-spec.json |
| Netlify | `netlify` | https://raw.githubusercontent.com/netlify/open-api/master/swagger.yml |
| NinjaOne | `ninjaone` | https://app.ninjarmm.com/apidocs/NinjaRMM-API-v2.json |
| PagerDuty | `pagerduty` | https://raw.githubusercontent.com/PagerDuty/api-schema/main/reference/REST/openapiv3.json |
| SendGrid | `sendgrid` | https://raw.githubusercontent.com/twilio/sendgrid-oai/main/spec/json/tsg_mail_v3.json |
| Slack | `slack` | https://raw.githubusercontent.com/slackapi/slack-api-specs/master/web-api/slack_web_openapi_v2.json |
| Slide | `slide` | https://api.slide.tech/openapi.json |
| Square | `square` | https://raw.githubusercontent.com/square/connect-api-specification/master/api.json |
| Stripe | `stripe` | https://raw.githubusercontent.com/stripe/openapi/master/openapi/spec3.yaml |
| Syncro | `syncro` | https://api-docs.syncromsp.com/swagger.json |
| Twilio | `twilio` | https://raw.githubusercontent.com/twilio/twilio-oai/main/spec/json/twilio_api_v2010.json |
| UniFi | `unifi_*` | https://developer.ui.com/{network,protect,site-manager}/openapi.json |
| Vercel | `vercel` | https://openapi.vercel.sh/ |
| Xero | `xero` | https://raw.githubusercontent.com/XeroAPI/Xero-OpenAPI/master/xero_accounting.yaml |

Acronis publishes per-product OpenAPI under `https://developer.acronis.com/doc/`. Reddit Ads is `https://ads-api.reddit.com/api/v3/openapi.json`.

Instance copies:

- HaloPSA also serves `{instance}/api/swagger/v2/swagger.json`.
- N-central serves Swagger on the appliance at `{fqdn}/api-explorer`.
- UniFi Network on a local console: `{console}/proxy/network/api-docs/integration.json`.
- Datto RMM platforms share one schema. Pinotage answered 200. Merlot, Vidal, Concord and Zinfandel answered 500 on the same path the day this was checked. Swagger UI: `https://{platform}-api.centrastage.net/api/swagger-ui/index.html`.

## GraphQL endpoints

One route, one verb: the connector compiles the schema the endpoint answers with, not a
document. `kind: graphql` on the catalog; `endpoint_template` carries the tenant or instance
placeholder, which is filled at import and never sent by this repository; `introspection`
says whether the endpoint answers an introspection query without a token (`open`), with one
(`auth`), or has it closed (`closed`, open it once on the instance). A per-instance product
such as Unraid (`{instance}/graphql`, unraid-api on OS 7.2+, introspection open) is imported
with `--kind graphql` against the instance.

114 GraphQL catalogs:

| Product | Catalog | Endpoint | Introspection |
|---|---|---|---|
| Adobe Commerce | `adobe_commerce_graphql` | `https://{store_domain}/graphql` | unknown |
| Amplience GraphQL Management | `amplience` | see `source` | open |
| Apollo GraphOS Platform | `apollo_graphos` | see `source` | open |
| Appwrite | `appwrite` | see `source` | open |
| Ashby | `ashby_graphql` | see `source` | auth |
| BigCommerce | `bigcommerce_account_graphql` | `https://api.bigcommerce.com/accounts/{account_uuid}/graphql` | auth |
| Bitquery Streaming | `bitquery` | see `source` | auth |
| Braintree | `braintree_2` | see `source` | auth |
| Brandfolder | `brandfolder` | see `source` | unknown |
| Builder.io GraphQL Content | `builder_io` | `https://cdn.builder.io/api/v3/graphql/{api_key}` | unknown |
| Buildkite | `buildkite_graphql` | see `source` | auth |
| Canvas LMS | `canvas_lms` | `https://{instance}.instructure.com/api/graphql` | auth |
| Catalysis-Hub | `catalysis_hub` | see `source` | open |
| Cato Networks | `cato_networks` | see `source` | open |
| Cloudflare | `cloudflare_analytics_graphql` | see `source` | auth |
| commercetools | `commercetools` | `https://api.{region}.commercetools.com/{projectKey}/graphql` | auth |
| Contentful | `contentful_delivery_graphql` | `https://graphql.contentful.com/content/v1/spaces/{space_id}/environments/{environment}` | auth |
| Contentstack GraphQL Content Delivery | `contentstack` | `https://graphql.contentstack.com/stacks/{stack_api_key}?environment={environment}` | auth |
| Countries | `countries_trevorblades` | see `source` | open |
| Courier | `courier` | see `source` | auth |
| Craft CMS | `craft_cms` | `https://{host}/actions/graphql/api` | unknown |
| CrowdStrike Falcon | `crowdstrike_logscale_graphql` | see `source` | closed |
| Crystallize Catalogue | `crystallize` | `https://api.crystallize.com/{tenant}/catalogue` | open |
| DatoCMS Content Delivery | `datocms` | see `source` | auth |
| dbt Cloud Discovery | `dbt_discovery` | see `source` | open |
| dbt Semantic Layer | `dbt_semantic_layer` | see `source` | open |
| Digitransit Routing | `digitransit` | see `source` | auth |
| Directus | `directus` | `https://{host}/graphql` | unknown |
| dotCMS | `dotcms` | `https://{host}/api/v1/graphql` | unknown |
| EAN-Search | `ean_search` | see `source` | auth |
| EHRI Portal | `ehri` | see `source` | open |
| Entur Journey Planner v3 | `entur_journey_planner` | see `source` | open |
| Escape platform GraphQL backend | `escape_tech` | see `source` | open |
| Expo EAS | `expo_eas` | see `source` | open |
| Fireflies | `fireflies_ai_graphql` | see `source` | auth |
| Fly.io | `fly_io_graphql` | see `source` | open |
| Fragment Ledger | `fragment` | `https://api.fragment.dev/graphql` | auth |
| Frontify | `frontify` | `https://{domain}.frontify.com/graphql` | unknown |
| GitHub GraphQL API (v4) | `github_2` | see `source` | auth |
| GitLab | `gitlab_graphql` | see `source` | open |
| GraphQL Hive | `graphql_hive` | see `source` | open |
| GraphQL Pokemon | `graphql_pokemon` | see `source` | open |
| GraphQLZero | `graphqlzero` | see `source` | open |
| HackerOne | `hackerone` | see `source` | closed |
| Hashnode Public | `hashnode` | see `source` | unknown |
| Highnote | `highnote` | see `source` | auth |
| HubSpot | `hubspot_graphql_graphql` | see `source` | auth |
| Hygraph (formerly GraphCMS) | `hygraph` | `https://{region}.cdn.hygraph.com/v2/{projectId}/{environment}` | unknown |
| Jira Cloud | `atlassian_graphql_graphql` | see `source` | auth |
| JupiterOne | `jupiterone` | see `source` | auth |
| KeystoneJS | `keystonejs` | `https://{host}/api/graphql` | unknown |
| Kibo Commerce Storefront | `kibo_commerce` | `https://{tenant}.mozu.com/graphql` | auth |
| Kontent.ai Delivery | `kontent_ai` | `https://graphql.kontent.ai/{environment_id}` | unknown |
| Lansweeper Data | `lansweeper` | see `source` | auth |
| LeetCode | `leetcode` | see `source` | closed |
| Linear | `linear_graphql` | see `source` | open |
| melodyRepo | `melody_repo` | see `source` | open |
| monday.com | `monday_graphql` | see `source` | auth |
| Nacelle Storefront | `nacelle` | `https://storefront.api.nacelle.com/graphql/v1/spaces/{space_id}` | open |
| NASA Earthdata | `nasa_earthdata` | see `source` | open |
| Nautobot | `nautobot` | `https://{instance}/api/graphql/` | unknown |
| NetBox | `netbox` | `https://{instance}/graphql/` | unknown |
| New Relic | `new_relic_nerdgraph_graphql` | see `source` | auth |
| Octopus Energy Kraken | `octopus_energy` | see `source` | open |
| Open Collective GraphQL API v2 | `open_collective` | see `source` | open |
| Open Targets Platform | `open_targets` | see `source` | open |
| OpsLevel | `opslevel_graphql` | see `source` | auth |
| Optimizely Graph (Content Graph) | `optimizely_graph` | see `source` | auth |
| Panther | `panther` | `https://api.{panther_domain}.runpanther.net/public/graphql` | auth |
| Payload CMS | `payload_cms` | `https://{host}/api/graphql` | unknown |
| Pipefy | `pipefy` | see `source` | auth |
| Platzi Fake Store | `platzi_fake_store` | see `source` | open |
| PokeAPI | `pokeapi_graphql` | see `source` | open |
| Prismic | `prismic` | `https://{repo}.cdn.prismic.io/graphql` | unknown |
| Product Hunt API v2 | `product_hunt` | see `source` | auth |
| Railway | `railway` | see `source` | open |
| Railway | `railway_graphql` | see `source` | open |
| Rewst platform | `rewst` | see `source` | open |
| Rick and Morty | `rick_and_morty` | see `source` | open |
| Saleor | `saleor` | see `source` | open |
| Sanity | `sanity_graphql` | `https://{projectId}.api.sanity.io/v{version}/graphql/{dataset}/{tag}` | auth |
| ServiceNow | `servicenow_graphql_graphql` | `https://{instance}.service-now.com/api/now/graphql` | unknown |
| Shopify | `shopify_admin_graphql` | `https://{store}.myshopify.com/admin/api/2026-07/graphql.json` | auth |
| Silverstripe CMS | `silverstripe` | `https://{host}/graphql` | unknown |
| Sitecore Experience Edge Delivery | `sitecore_experience_edge` | see `source` | auth |
| Slack | `salesforce_graphql_graphql` | `https://{myDomain}.my.salesforce.com/services/data/v{version}/graphql` | auth |
| Sourcegraph | `sourcegraph` | see `source` | open |
| Spacelift | `spacelift` | `https://{account}.app.spacelift.io/graphql` | open |
| SpaceX GraphQL API (community mirror) | `spacex_graphql` | see `source` | open |
| Stanford HIVDB | `hivdb_stanford` | see `source` | open |
| start.gg | `start_gg` | see `source` | auth |
| Statamic | `statamic` | `https://{host}/graphql` | unknown |
| Stitch | `stitch_money` | see `source` | closed |
| Storyblok GraphQL Content Delivery | `storyblok_graphql` | see `source` | open |
| Strapi | `strapi_graphql` | `https://{host}/graphql` | unknown |
| Supabase | `supabase_graphql_graphql` | `https://{project_ref}.supabase.co/graphql/v1` | auth |
| Swan Banking | `swan` | see `source` | open |
| SWAPI | `swapi_graphql` | see `source` | open |
| Swell Frontend | `swell` | `https://{store_id}.swell.store/graphql/v2` | unknown |
| Swop foreign exchange | `swop` | see `source` | auth |
| TCGdex | `tcgdex` | see `source` | open |
| The Graph Gateway | `the_graph` | `https://gateway.thegraph.com/api/{api_key}/subgraphs/id/{subgraph_id}` | auth |
| Tibber | `tibber` | see `source` | open |
| Twenty CRM | `twenty_crm` | see `source` | closed |
| Twingate | `twingate` | `https://{network}.twingate.com/api/graphql/` | unknown |
| Umbraco Heartcore | `umbraco_heartcore` | see `source` | auth |
| Unraid | `unraid` | `{instance}/graphql` | open |
| Vendure Shop & Admin | `vendure` | see `source` | open |
| VTEX IO GraphQL APIs | `vtex_io` | see `source` | unknown |
| Wave Business | `wave_accounting` | see `source` | open |
| Wiz | `wiz_graphql` | `https://api.{region}.app.wiz.io/graphql` | auth |
| WPGraphQL | `wpgraphql` | `https://{host}/graphql` | unknown |
| Yelp Fusion | `yelp_fusion` | see `source` | auth |
| Zendesk | `zendesk_sell_graphql` | see `source` | auth |

## Login-gated documents

Tagged `account`. `import` from `source` will not work unauthenticated. Download the document from the vendor portal (or export it from the instance) and import that file.

| Where it lives | Products |
|---|---|
| developer.connectwise.com (sign-in) | ConnectWise PSA (`All.json`, split into the 12 PSA catalogs), RMM (`currentPartnerAPI.yaml`), Automate (ReDoc zip), CPQ (`SellAPI.json`) |
| Tenant console | SentinelOne `/api-doc/`, Atera `https://app.atera.com/apidocs` (401 without a key) |
| Instance | Hudu, Unraid (`/graphql`, 7.2+), Home Assistant, Proxmox, TrueNAS, Synology, Portainer `{portainer}/api/docs`, HaloPSA, N-central |
| Partner / developer portal | CrowdStrike, Palo Alto, Tenable, Qualys, Rapid7, Wiz, Pax8, ThreatLocker, Action1, Addigy, Liongard, ImmyBot, Mosyle, Domotz, Kaseya VSA and BMS |
| Account or OAuth app | Okta, JumpCloud, Duo, Auth0, Entra, Intune, Salesforce, HubSpot, Zendesk, ServiceNow, Datadog, New Relic, Splunk, Elastic |
| Accounting OAuth | QuickBooks, Xero, NetSuite, Sage, Gusto, FreshBooks |
| Cloud account | AWS, Azure, Google Cloud |

ScreenConnect has no public OpenAPI. The connector ships an authored `session-manager.yaml` (RPC). SuperOps is GraphQL at https://developer.superops.ai/. IT Glue's `GET .../swagger.json` is 403; the HTML JSON:API docs are public, the spec is not.

Filter the full set with:

```bash
jq '.products[] | select(.tags | index("account")) | .name' index.json
```

## Checked

Rechecked on 2026-09-10 with the same methods: Grafbase and Shopify no longer answer an
`initialize` on their MCP URL (404) and left the MCP table; Pennylane moved to `/mcp`;
Datto RMM and SendGrid point at the documents that actually answer.

Every URL in this file was probed on 2026-09-09, and the method matters: a GET on an
MCP endpoint is meaningless, so the MCP URLs were probed with a real `initialize`
POST and the GraphQL endpoints with a real introspection query.

| What | Probed | Result |
|---|---|---|
| MCP endpoints | 100 (the set before this pass) | 94 alive: 71 answer the OAuth challenge, 17 answer JSON-RPC unauthenticated, 6 want auth without a challenge. 2 gone (Grafbase, Shopify), 3 redirect, 1 was 503 |
| GraphQL endpoints | 76 that are not per-tenant | 74 alive: 36 with introspection open, 24 behind auth, the rest answering. 2 repaired to the vendor page |
| Per-tenant GraphQL | 36 | Never shipped as a `source`: the vendor page is the source and the template moved to `endpoint_template` |
| Catalog sources | 454 (the set before this pass) | 32 returned a machine document, 326 a vendor page (which is what the `account` tier is), and 60 were dead or erroring |
| The 60 dead ones | each one chased to its current home | 16 now point at a real OpenAPI or Swagger document that did not exist in this file before, 39 at a live vendor page, 3 at a page that is alive in a browser and 403s to fetchers (`bot_blocked`), and 2 vendors publish no API document any more (`no_public_document`) |

A GET on an MCP endpoint answers 401, 405 or 406 on a perfectly healthy server, and a
GET on a GraphQL endpoint answers 400 or 405. Any count of dead servers built from GET
probes is wrong, including one this repository published before this pass.

Rebrands the repair pass turned up, now reflected in the file: Datto RMM's swagger
group was renamed (`Datto-RMM` to `Datto-RMM-v2`, which is why every region host
answered 500), SendGrid's spec moved org and split into about thirty per-product
documents, Timescale is TigerData, Datto Commerce is Kaseya Quote Manager, StreamOne
Ion is StreamOne Stellr, Malwarebytes business is ThreatDown, and Cylance is Arctic
Wolf Aurora, which publishes no REST reference at all any more.

## Limits

- Microsoft Graph v1.0 is about 44 MiB, Cloudflare about 18 MiB, GitHub about 10 MiB. The connector YAML node budget currently refuses the Graph compile. The hub still lists them so an operator can split the document or wait on a later compiler.
- `hub_url` is capped at 256 KiB. PRs that push `index.json` over that fail closed at fetch.
- `mark` is two characters and must be unique. Letters or a letter plus a digit (`S1`, `B2`) are fine.
- Catalog `id` values must be unique across the file, not only inside one product.

## Not listed

No OpenAPI, Swagger, GraphQL, RPC document or official MCP found:

- **CIPP** (CyberDrain Improved Partner Portal): instance REST, no published spec.
- **Apple HomeKit**: HAP / Matter accessory protocol. Not a document the connector can import.

Open an issue if that changes.

## Contribute

**Missing vendor.** Open an issue with the [Add a product](https://github.com/pulsatrixtechnologies/connector-hub/issues/new?template=add-product.md) template. Vendor name, public document URL if any, whether a login is required, one sentence on what an MSP uses it for. No keys, tokens, tenant hostnames or company names.

**Pull request.** Keep `pulsatrix-hub/1`. Unique `id` and `mark`. Every catalog `source` an `https://` URL. `mcp.url` only on the vendor's own domain (or the documented regional host). Do not add community wrappers. Stay under 256 KiB. Do not invent MCP URLs.

The console copies this index. A broken `source` is worse than a missing card.

## Status

Public. Maintained by [Pulsatrix Technologies Inc.](https://pulsatrix.ca). The console ships a bundled copy of this index; `hub_url` can point at `main` or at a fork.

| | |
|---|---|
| Format | `pulsatrix-hub/1` |
| Index | [`index.json`](index.json) on `main` |
| Cap | 256 KiB (`hub_url` fetch) |
| Issues | [Add a product](https://github.com/pulsatrixtechnologies/connector-hub/issues/new?template=add-product.md) |

## License

Copyright Pulsatrix Technologies Inc. The index lists public vendor URLs and developer pages. Product names and marks belong to their vendors.

See [pulsatrix.ca/terms](https://pulsatrix.ca/terms) and [pulsatrix.ca/privacy](https://pulsatrix.ca/privacy). Contact: [hello@pulsatrix.ca](mailto:hello@pulsatrix.ca).
