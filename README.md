# Pulsatrix connector hub

A public registry of products the Pulsatrix connector can add: official vendor MCP servers first, then OpenAPI, Swagger, GraphQL and RPC documents compiled to catalogs.

The console starts empty. An operator opens Products, picks a card and taps Add (or opens the vendor MCP). Profiles then grant that product to people.

`index.json` is `pulsatrix-hub/1`. The gateway fetch cap for `hub_url` is 256 KiB. This file stays under that.

The `account` tag is the login-gated path. The Add a product issue template already asks for a public document URL and whether a login is required. Those products stay in `index.json` so an operator who holds the vendor document can import it.

## This repository

| File | What it is |
|---|---|
| `index.json` | `pulsatrix-hub/1`: name, summary, tags, default catalogs, `source` on each catalog, optional `mcp` |
| `.github/ISSUE_TEMPLATE/add-product.md` | how to ask for a missing vendor |

The compiled catalogs themselves live in the connector repo (`catalog/`). The hub is the index the console shows, not a second copy of every document.

Each catalog may carry `source`: a direct URL to the OpenAPI, Swagger, GraphQL or RPC document when one exists. Public files are the fetchable spec. Login-gated products point at the vendor developer page. The operator still imports the document they hold.

A product may also carry `mcp`: the vendor's own MCP server. **Use that first.** Compile the OpenAPI catalog into Pulsatrix only when you need profiles, journal and scopes on this host.

Counts in this revision: 426 products, 100 with a vendor-hosted MCP URL, 6 with official MCP docs only (local, self-hosted, or per-tenant URL).

## Tags

- `mcp` official vendor MCP. Connect this before compiling the API
- `rest` REST API
- `openapi` OpenAPI
- `swagger` Swagger
- `graphql` GraphQL
- `rpc` RPC methods
- `account` needs a login or a vendor document the operator already has
- `private` private API (the vendor's own site, with the client's authorisation)

## Official MCP first

These products ship a vendor-hosted MCP. Point the AI client at `mcp.url`. The `source` OpenAPI stays as a fallback for a Pulsatrix catalog.

Community wrappers and aggregator MCP endpoints are not listed. If the vendor later hosts one, add `mcp` and the `mcp` tag.

| Product | MCP |
|---|---|
| Ahrefs | https://api.ahrefs.com/mcp/mcp |
| Airtable | https://mcp.airtable.com/mcp |
| Amazon Web Services | https://aws-mcp.us-east-1.api.aws/mcp |
| Amplitude | https://mcp.amplitude.com/mcp |
| Apify | https://mcp.apify.com |
| Asana | https://mcp.asana.com/mcp |
| Ashby | https://mcp.ashbyhq.com/mcp/v1 |
| Astro Docs | https://mcp.docs.astro.build/mcp |
| Attio | https://mcp.attio.com/mcp |
| AWS Knowledge | https://knowledge-mcp.global.api.aws |
| Bitbucket Cloud | https://mcp.atlassian.com/v2/mcp |
| Box | https://mcp.box.com |
| Buildkite | https://mcp.buildkite.com/mcp |
| Canva | https://mcp.canva.com/mcp |
| Clay | https://mcp.clay.earth/mcp |
| ClickUp | https://mcp.clickup.com/mcp |
| Close | https://mcp.close.com/mcp |
| Cloudflare | https://mcp.cloudflare.com/mcp |
| Cloudflare Observability | https://observability.mcp.cloudflare.com/sse |
| Cloudflare Radar | https://radar.mcp.cloudflare.com/sse |
| Cloudflare Workers Bindings | https://bindings.mcp.cloudflare.com/sse |
| Cloudinary | https://asset-management.mcp.cloudinary.com/sse |
| Coda | https://coda.io/apis/mcp |
| Confluence Cloud | https://mcp.atlassian.com/v2/mcp |
| Context7 | https://mcp.context7.com/mcp |
| Cortex | https://mcp.cortex.io/mcp |
| DeepWiki | https://mcp.deepwiki.com/mcp |
| draw.io | https://mcp.draw.io/mcp |
| Egnyte | https://mcp-server.egnyte.com/sse |
| Exa | https://mcp.exa.ai/mcp |
| Excalidraw | https://mcp.excalidraw.com |
| Figma | https://mcp.figma.com/mcp |
| Fireflies | https://api.fireflies.ai/mcp |
| GitHub | https://api.githubcopilot.com/mcp/ |
| GitLab | https://gitlab.com/api/v4/mcp |
| Globalping | https://mcp.globalping.dev/sse |
| Google BigQuery | https://bigquery.googleapis.com/mcp |
| Google Compute Engine | https://compute.googleapis.com/mcp |
| Google Kubernetes Engine | https://container.googleapis.com/mcp |
| Google Maps | https://mapstools.googleapis.com/mcp |
| Grafbase | https://api.grafbase.com/mcp |
| Hex | https://app.hex.tech/mcp |
| Honeycomb | https://mcp.honeycomb.io/mcp |
| HubSpot | https://mcp.hubspot.com |
| Hugging Face | https://hf.co/mcp |
| Indeed | https://mcp.indeed.com/claude/mcp |
| Intercom | https://mcp.intercom.com/sse |
| Jam | https://mcp.jam.dev/mcp |
| Jira Cloud | https://mcp.atlassian.com/v2/mcp |
| Jira Service Management | https://mcp.atlassian.com/v2/mcp |
| Linear | https://mcp.linear.app/mcp |
| Malware Patrol | https://mcp.malwarepatrol.net/v1 |
| Mercado Libre | https://mcp.mercadolibre.com/mcp |
| Mercado Pago | https://mcp.mercadopago.com/mcp |
| Microsoft Graph | https://mcp.svc.cloud.microsoft/enterprise |
| Microsoft Learn | https://learn.microsoft.com/api/mcp |
| Miro | https://mcp.miro.com/ |
| Mixpanel | https://mcp.mixpanel.com/mcp |
| monday.com | https://mcp.monday.com/sse |
| Neon | https://mcp.neon.tech/mcp |
| Netlify | https://netlify-mcp.netlify.app/mcp |
| Notion | https://mcp.notion.com/mcp |
| OpenZeppelin | https://mcp.openzeppelin.com/contracts/solidity/mcp |
| PagerDuty | https://mcp.pagerduty.com/mcp |
| Parallel Search | https://search-mcp.parallel.ai/mcp |
| Parallel Task | https://task-mcp.parallel.ai/mcp |
| PayPal | https://mcp.paypal.com/mcp |
| Pennylane | https://app.pennylane.com/mcp/messages |
| Pipedrive | https://mcp.pipedrive.ai/mcp |
| Plaid | https://api.dashboard.plaid.com/mcp/sse |
| Port | https://mcp.port.io/v1 |
| PostHog | https://mcp.posthog.com/mcp |
| Postman | https://mcp.postman.com/minimal |
| Prisma Postgres | https://mcp.prisma.io/mcp |
| Ramp | https://ramp-mcp-remote.ramp.com/mcp |
| Render | https://mcp.render.com/mcp |
| Replicate | https://mcp.replicate.com/sse |
| Sanity | https://mcp.sanity.io |
| Semgrep | https://mcp.semgrep.ai/mcp |
| Semrush | https://mcp.semrush.com/v1/mcp |
| Sentry | https://mcp.sentry.dev/mcp |
| Shopify | https://mcp.shopify.com/mcp |
| SISTRIX | https://api.sistrix.com/mcp/ |
| Slack | https://mcp.slack.com/mcp |
| Spendesk | https://public-api.spendesk.com/v1/mcp |
| Square | https://mcp.squareup.com/sse |
| Stack Overflow | https://mcp.stackoverflow.com |
| Statista | https://api.statista.ai/v1/mcp |
| Stripe | https://mcp.stripe.com |
| Stytch | https://mcp.stytch.dev/mcp |
| Supabase | https://mcp.supabase.com/mcp |
| Telnyx | https://api.telnyx.com/v2/mcp |
| ThoughtSpot | https://agent.thoughtspot.app/mcp |
| Vercel | https://mcp.vercel.com/ |
| Webflow | https://mcp.webflow.com/sse |
| Wix | https://mcp.wix.com/mcp |
| Wolfram | https://agenttools.wolfram.com/mcp |
| X | https://api.x.com/mcp |
| X Docs | https://docs.x.com/mcp |
| Zapier | https://mcp.zapier.com/api/mcp/mcp |

PagerDuty also serves the EU at `https://mcp.eu.pagerduty.com/mcp`. Atlassian Rovo MCP (`https://mcp.atlassian.com/v2/mcp`) covers Jira, Confluence, Bitbucket and Jira Service Management. Prefer `/mcp` (Streamable HTTP) over `/sse` when the vendor publishes both.

Official MCP with no single public URL (local server, self-hosted, or per-account):

| Product | Docs |
|---|---|
| Databricks | https://docs.databricks.com/aws/en/generative-ai/mcp/ |
| Google Cloud | https://docs.cloud.google.com/mcp/overview |
| Grafana | https://grafana.com/docs/grafana/latest/developer-resources/mcp/ |
| Microsoft Azure | https://learn.microsoft.com/en-us/azure/developer/azure-mcp-server/overview |
| MongoDB Atlas | https://www.mongodb.com/docs/mcp-server/ |
| Snowflake | https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp |

## Public documents

These `source` URLs return the spec without a vendor login (checked 2026-09-08, plus later additions).

| Product | Catalog | Source |
|---|---|---|
| AlertOps | `alertops` | https://api.alertops.com/swagger/v1/swagger.json |
| Auvik | `auvik` | https://auvikapi.us1.my.auvik.com/spec |
| Acronis | `acronis_*` | https://developer.acronis.com/doc/.../openapi.json |
| Axcient x360 | `axcient_*` | https://developer.axcient.com/specs/{product}.yaml |
| Cisco Meraki | `cisco_meraki_*` | https://raw.githubusercontent.com/meraki/openapi/master/openapi/spec3.json |
| Autotask PSA | `autotask` | https://webservices.autotask.net/atservicesrest/swagger/docs/v1 |
| Huntress | `huntress` | https://api.huntress.io/v1/swagger_doc.json |
| NinjaOne | `ninjaone` | https://app.ninjarmm.com/apidocs/NinjaRMM-API-v2.json |
| Slide | `slide` | https://api.slide.tech/openapi.json |
| Syncro | `syncro` | https://api-docs.syncromsp.com/swagger.json |
| Reddit Ads | `reddit_ads` | https://ads-api.reddit.com/api/v3/openapi.json |
| HaloPSA | `halo_psa` | https://halo.haloservicedesk.com/api/swagger/v2/swagger.json |
| Datto RMM | `datto_rmm` | https://pinotage-api.centrastage.net/api/v3/api-docs/Datto-RMM |
| N-able N-central | `n_central` | https://documentation.n-able.com/N-central/preview/rest_api_preview/Content/Resources/swaggerapi/dist/openapi-spec.json |
| PagerDuty | `pagerduty` | https://raw.githubusercontent.com/PagerDuty/api-schema/main/reference/REST/openapiv3.json |
| Xero | `xero` | https://raw.githubusercontent.com/XeroAPI/Xero-OpenAPI/master/xero_accounting.yaml |
| Microsoft Graph | `microsoft_graph` | https://raw.githubusercontent.com/microsoftgraph/msgraph-metadata/master/openapi/v1.0/openapi.yaml |
| UniFi | `unifi_*` | https://developer.ui.com/{network,protect,site-manager}/.../openapi.json |
| Grafana | `grafana` | https://raw.githubusercontent.com/grafana/grafana/main/public/api-merged.json |
| Stripe | `stripe` | https://raw.githubusercontent.com/stripe/openapi/master/openapi/spec3.yaml |
| Square | `square` | https://raw.githubusercontent.com/square/connect-api-specification/master/api.json |
| Cloudflare | `cloudflare` | https://raw.githubusercontent.com/cloudflare/api-schemas/main/openapi.yaml |
| DigitalOcean | `digitalocean` | https://raw.githubusercontent.com/digitalocean/openapi/main/specification/DigitalOcean-public.v2.yaml |
| Slack | `slack` | https://raw.githubusercontent.com/slackapi/slack-api-specs/master/web-api/slack_web_openapi_v2.json |
| Twilio | `twilio` | https://raw.githubusercontent.com/twilio/twilio-oai/main/spec/json/twilio_api_v2010.json |
| GitHub | `github` | https://raw.githubusercontent.com/github/rest-api-description/main/descriptions/api.github.com/api.github.com.yaml |
| GitLab | `gitlab` | https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/api/openapi/openapi_v2.yaml |
| Jira Cloud | `jira` | https://developer.atlassian.com/cloud/jira/platform/swagger-v3.v3.json |
| Asana | `asana` | https://raw.githubusercontent.com/Asana/openapi/master/defs/asana_oas.yaml |
| Bitbucket Cloud | `bitbucket` | https://api.bitbucket.org/swagger.json |
| CircleCI | `circleci` | https://circleci.com/api/v2/openapi.json |
| Kubernetes | `kubernetes` | https://raw.githubusercontent.com/kubernetes/kubernetes/master/api/openapi-spec/swagger.json |
| LaunchDarkly | `launchdarkly` | https://app.launchdarkly.com/api/v2/openapi.json |
| Netlify | `netlify` | https://raw.githubusercontent.com/netlify/open-api/master/swagger.yml |
| SendGrid | `sendgrid` | https://raw.githubusercontent.com/sendgrid/sendgrid-oai/main/oai.json |
| Vercel | `vercel` | https://openapi.vercel.sh/ |

Notes on public files:

- Datto RMM platforms share one schema. Pinotage answered 200. Merlot, Vidal, Concord and Zinfandel answered 500 on the same path the day this was checked. Swagger UI: `https://{platform}-api.centrastage.net/api/swagger-ui/index.html`.
- HaloPSA is instance-hosted. The Halo Service Desk demo spec is public. A tenant copy also lives at `{instance}/api/swagger/v2/swagger.json`.
- N-central also ships Swagger on the appliance at `{fqdn}/api-explorer`.
- UniFi Network, Protect and Site Manager OpenAPI files are public on developer.ui.com. A local console also serves Network at `{console}/proxy/network/api-docs/integration.json`.
- Microsoft Graph v1.0 is about 44 MiB, Cloudflare about 18 MiB, GitHub about 10 MiB. The connector YAML node budget currently refuses the Graph compile. The hub still lists them so an operator can split or wait on a later compiler.

## Login-gated documents

Tagged `account`. The spec is behind a developer login, a tenant console, or an instance Swagger page. Do not expect `import` from the `source` URL to work unauthenticated. Download from the vendor portal (or export from the instance) and import that file.

Examples (not exhaustive; filter `index.json` on the `account` tag for the full set):

| Product | Where the document is |
|---|---|
| ConnectWise PSA / RMM / Automate / CPQ | developer.connectwise.com, sign-in |
| ScreenConnect | vendor developer docs; connector ships `session-manager.yaml` |
| SentinelOne | tenant console `/api-doc/` |
| Atera | https://app.atera.com/apidocs (401 without a key) |
| SuperOps | GraphQL at https://developer.superops.ai/ |
| Freshdesk, Freshservice | vendor developer, HTML reference |
| Hudu, IT Glue | instance or partner login |
| Pax8, ThreatLocker, Action1, Addigy, Liongard | partner / in-app docs |
| ImmyBot, Mosyle, Domotz, Kaseya VSA / BMS | vendor developer / help |
| CrowdStrike, Palo Alto, Tenable, Qualys, Rapid7, Wiz | partner / developer portal |
| Datadog, New Relic, Dynatrace, Splunk, Elastic | account |
| Okta, JumpCloud, Duo, Auth0, Entra, Intune | account or OAuth app |
| Salesforce, HubSpot, Zendesk, ServiceNow | account or OAuth app |
| Unraid, Home Assistant, Proxmox, TrueNAS, Synology | instance |
| QuickBooks, Xero, NetSuite, Sage, Gusto | accounting OAuth |
| AWS, Azure, Google Cloud | cloud account |

Community unofficial specs exist for some gated APIs. They are not the vendor document. The hub `source` stays on the official page.

## Referenced, no published OpenAPI

Products seen in the wild with no OpenAPI, Swagger, GraphQL schema, RPC document or official MCP found for the hub:

- CIPP (CyberDrain Improved Partner Portal): instance REST, no published spec
- Apple HomeKit: accessory protocol (HAP / Matter), not a REST or GraphQL document an operator can import

Ask for those with the Add a product template if a document appears. ClickUp now has an official MCP (`https://mcp.clickup.com/mcp`) and is in `index.json`.

## Request a product

Open an issue with the Add a product template. Include the vendor name, a public document URL if there is one, the official MCP URL if the vendor hosts one, and whether a login is required. Do not attach keys, tokens or tenant hostnames.

Do not invent MCP URLs. Only `mcp.url` values the vendor publishes on their own domain (or the documented regional endpoint) belong here.

## Use it from a connector

The gateway embeds this index. An operator can point `hub_url` at a fork:

```
https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json
```
