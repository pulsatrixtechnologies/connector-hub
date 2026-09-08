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
  <a href="index.json"><img src="https://img.shields.io/badge/products-426-3c76f4?style=flat-square&labelColor=111a2f" alt="426 products"></a>
  <a href="#remote-mcp"><img src="https://img.shields.io/badge/MCP-100%20remote-3c76f4?style=flat-square&labelColor=111a2f" alt="100 remote MCP"></a>
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

426 products. 100 with a vendor-hosted MCP URL. 6 with official MCP docs only (local, self-hosted, or per-account). The gateway refuses a `hub_url` larger than 256 KiB. This file stays under that.

## Contents

1. [Use it](#use-it)
2. [What this repository is](#what-this-repository-is)
3. [Format](#format)
4. [MCP or catalog](#mcp-or-catalog)
5. [Tags](#tags)
6. [What belongs here](#what-belongs-here)
7. [Remote MCP](#remote-mcp)
8. [Fetchable specs](#fetchable-specs)
9. [Login-gated documents](#login-gated-documents)
10. [Limits](#limits)
11. [Not listed](#not-listed)
12. [Contribute](#contribute)
13. [Status](#status)
14. [License](#license)

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
| `mcp` | no | Official vendor MCP. See below. |

### Catalog

| Field | Required | Notes |
|---|---|---|
| `id` | yes | Unique across the whole index. |
| `title` | yes | Short label on the card. |
| `file` | yes | Filename the connector looks up under `catalog/` (`pagerduty.json`). |
| `source` | no | HTTPS URL of the OpenAPI, Swagger, GraphQL or RPC document, or of the vendor developer page when the file is login-gated. |

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
| `mcp` | Official vendor MCP. Connect it before compiling the API. |
| `rest` | HTTP REST. |
| `openapi` | OpenAPI document. |
| `swagger` | Swagger 2 document. |
| `graphql` | GraphQL schema or endpoint. |
| `rpc` | RPC methods (JSON-RPC or similar). |
| `account` | Spec is behind a login, a tenant console, or an instance page. `source` is the vendor page, not a fetchable file. |
| `private` | Private API: the vendor's own site, with the client's authorisation. |

Filter in the console or with `jq`. Combining `mcp` and `account` is normal: the MCP may be public while the OpenAPI is not.

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
| Pennylane | https://app.pennylane.com/mcp/messages |
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
| Shopify | https://mcp.shopify.com/mcp |
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
| Grafbase | https://api.grafbase.com/mcp |
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
| Datto RMM | `datto_rmm` | https://pinotage-api.centrastage.net/api/v3/api-docs/Datto-RMM |
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
| SendGrid | `sendgrid` | https://raw.githubusercontent.com/sendgrid/sendgrid-oai/main/oai.json |
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
