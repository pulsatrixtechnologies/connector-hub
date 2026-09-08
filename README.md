# Pulsatrix connector hub

A public list of products the Pulsatrix connector can add: OpenAPI, Swagger, GraphQL, RPC and similar API documents, compiled to catalogs.

The console starts empty. An operator opens Products, picks a card (ConnectWise Platform, SentinelOne, AlertOps, ...) and taps Add. Profiles then grant that product to people.

The `account` tag is the login-gated path. The Add a product issue template already asks for a public document URL and whether a login is required. Those products stay in `index.json` so an operator who holds the vendor document can import it.

## This repository

| File | What it is |
|---|---|
| `index.json` | `pulsatrix-hub/1`: name, summary, tags, default catalogs, `source` on each catalog |
| `.github/ISSUE_TEMPLATE/add-product.md` | how to ask for a missing vendor |

The compiled catalogs themselves live in the connector repo (`catalog/`). The hub is the index the console shows, not a second copy of every document.

Each catalog may carry `source`: a direct URL to the OpenAPI, Swagger, GraphQL or RPC document when one exists. Public files are the fetchable spec. Login-gated products point at the vendor developer page. The operator still imports the document they hold.

A product may also carry `mcp`: the vendor's own MCP server. **Use that first.** Compile the OpenAPI catalog into Pulsatrix only when you need profiles, journal and scopes on this host.

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

| Product | MCP | Docs |
|---|---|---|
| Stripe | https://mcp.stripe.com | https://docs.stripe.com/mcp |
| Slack | https://mcp.slack.com/mcp | https://docs.slack.dev/ai/slack-mcp-server |
| GitHub | https://api.githubcopilot.com/mcp/ | https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md |
| Jira Cloud | https://mcp.atlassian.com/v2/mcp | Atlassian Rovo MCP |
| Linear | https://mcp.linear.app/mcp | https://linear.app/docs/mcp |
| HubSpot | https://mcp.hubspot.com | HubSpot apps MCP |
| PayPal | https://mcp.paypal.com/mcp | PayPal MCP |
| Square | https://mcp.squareup.com/sse | Square MCP |
| PagerDuty | https://mcp.pagerduty.com/mcp | https://developer.pagerduty.com/docs/mcp-tooling-remote-server |
| GitLab | https://gitlab.com/api/v4/mcp | GitLab MCP server (instance URL on self-managed) |
| Microsoft Graph | https://mcp.svc.cloud.microsoft/enterprise | Microsoft MCP Server for Enterprise |
| Cloudflare | https://mcp.cloudflare.com/mcp | Cloudflare MCP servers |
| AWS | https://aws-mcp.us-east-1.api.aws/mcp | AWS MCP |
| Azure | n/a (local / self-hosted official server) | Azure MCP Server |
| Grafana | n/a (official local server) | Grafana MCP |

Community MCP wrappers are not listed. If the vendor later hosts one, add `mcp` and the `mcp` tag.

## Public documents

These `source` URLs return the spec without a vendor login (checked 2026-09-08).

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

Notes on public files:

- Datto RMM platforms share one schema. Pinotage answered 200. Merlot, Vidal, Concord and Zinfandel answered 500 on the same path the day this was checked. Swagger UI: `https://{platform}-api.centrastage.net/api/swagger-ui/index.html`.
- HaloPSA is instance-hosted. The Halo Service Desk demo spec is public. A tenant copy also lives at `{instance}/api/swagger/v2/swagger.json`.
- N-central also ships Swagger on the appliance at `{fqdn}/api-explorer`.
- UniFi Network, Protect and Site Manager OpenAPI files are public on developer.ui.com. A local console also serves Network at `{console}/proxy/network/api-docs/integration.json`.
- Microsoft Graph v1.0 is about 44 MiB, Cloudflare about 18 MiB, GitHub about 10 MiB. The connector YAML node budget currently refuses the Graph compile. The hub still lists them so an operator can split or wait on a later compiler.

## Login-gated documents

Tagged `account`. The spec is behind a developer login, a tenant console, or an instance Swagger page. Do not expect `import` from the `source` URL to work unauthenticated. Download from the vendor portal (or export from the instance) and import that file.

| Product | Where the document is | What you get |
|---|---|---|
| ConnectWise PSA | developer.connectwise.com, sign-in | `All.json`, split into the 12 PSA catalogs |
| ConnectWise RMM | developer.connectwise.com, sign-in | `currentPartnerAPI.yaml` (Asio) |
| ConnectWise Automate | developer.connectwise.com, sign-in | ReDoc zip (`swagger*.zip`) |
| ConnectWise CPQ | developer.connectwise.com, sign-in | `SellAPI.json` |
| ScreenConnect | vendor developer docs, no public OpenAPI | RPC methods. The connector ships an authored `session-manager.yaml` |
| SentinelOne | tenant console `/api-doc/` | Swagger 2 Management API |
| Atera | https://app.atera.com/apidocs (401 without a key) | OpenAPI 3 behind the in-app Swagger UI |
| SuperOps | https://developer.superops.ai/ | GraphQL, not REST |
| Freshdesk | https://developer.freshdesk.com/api/ | HTML reference, no public spec file found |
| Hudu | `{instance}` Admin, API Keys, View the API Documentation | Swagger on the instance |
| IT Glue | https://api.itglue.com/developer | HTML JSON:API docs. `GET .../swagger.json` is 403 |
| Pax8 | https://devx.pax8.com | OpenAPI per endpoint in ReadMe, no single public file |
| ThreatLocker | `https://portalapi.{instance}.threatlocker.com/swagger` | Unauthenticated `swagger.json` has empty `paths` |
| Action1 | https://app.action1.com/apidocs/ | OAS 3.1 Swagger UI. Raw spec URLs answered 403 |
| Addigy | in-app API v2 docs | Interactive docs, no public spec file found |
| Liongard | https://docs.liongard.com/reference | Partner docs. They mention a swagger file for partners |
| ImmyBot, Mosyle, Domotz, Kaseya VSA | vendor developer / help | Instance or partner login |
| Sophos Central | https://developer.sophos.com | Partner / Central login |
| KnowBe4, Proofpoint, Blackpoint, Blumira, Abnormal, Inforcer, RocketCyber, SaaS Alerts, usecure | vendor developer | Partner login |
| AFI.ai, CloudAlly, Backblaze Computer Backup, Datto BCDR, Unitrends, Spanning, Datto SaaS Protection | vendor developer | Partner login |
| BigLeaf | vendor developer | Account |
| Unraid | https://docs.unraid.net/API/ | GraphQL on the server (`/graphql`), 7.2+ built in |
| Home Assistant | https://developers.home-assistant.io/docs/api/rest/ | REST on the instance. No official OpenAPI file |
| UISP | Ubiquiti help | Partner / controller login |
| Proxmox VE | https://pve.proxmox.com/pve-docs/api-viewer/ | JSON schema in the API viewer, not a standalone OpenAPI file |
| TrueNAS, Synology DSM | vendor / instance docs | OpenAPI on the appliance for TrueNAS (`/api/v2.0`) |
| OPNsense, pfSense | vendor developer | Instance or XML-RPC |
| Portainer | instance Swagger | `{portainer}/api/docs` |
| PRTG, Zabbix, FortiGate | vendor manuals | HTTP API / JSON-RPC / FortiOS |
| Jamf Pro, Kandji, Apple Business Manager | vendor developer | MDM partner login |
| JumpCloud, Okta, Duo, Tailscale | vendor developer | Account or OAuth app |
| Bitwarden, 1Password, HashiCorp Vault | vendor developer | Public API or Connect, still needs a token |
| Harvest, Zoho Books, Sage, Sage Intacct, FreshBooks, NetSuite, Gusto, Clockify, PayPal | vendor developer | Accounting / payroll OAuth |
| Hetzner, AWS, Azure, Google Cloud, Linode, OVHcloud, Backblaze B2, Wasabi, Dropbox | vendor developer | Cloud account |
| Zoom, RingCentral, Zendesk, ServiceNow, Linear, Statuspage | vendor developer | Account or OAuth app |
| ScalePad, TimeZest, SmileBack, StreamOne, Sherweb, SalesBuildr, PandaDoc, Quote Manager | vendor developer | Account |
| 3CX, Better Stack, Rootly, runZero, HubSpot, QuickBooks Online, Google Workspace | vendor developer | Account or OAuth app |
| Crewhu, Mailprotector, Mimecast, IRONSCALES, SpamTitan, Nutanix, Clio, Kaseya BMS | vendor developer | Account |

Community unofficial specs exist for some gated APIs (Atera YAML in PSAtera, IT Glue YAML at jmaddington/ITG-Glue-OpenAPI, Hudu snapshots in n8n-nodes-hudu). They are not the vendor document. The hub `source` stays on the official page.

## Referenced, no published OpenAPI

MSP products seen in the wild with no OpenAPI, Swagger, GraphQL schema or RPC document found for the hub:

- CIPP (CyberDrain Improved Partner Portal): instance REST, no published spec
- Apple HomeKit: accessory protocol (HAP / Matter), not a REST or GraphQL document an operator can import
- Analytics 365, ClickUp, Warmly, PostHog, Alternative Payments, Microsoft Teams as a standalone channel (Teams itself is Microsoft Graph)

Ask for those with the Add a product template if a document appears.

## Request a product

Open an issue with the Add a product template. Include the vendor name, a public document URL if there is one, and whether a login is required. Do not attach keys, tokens or tenant hostnames.

## Use it from a connector

The gateway embeds this index. An operator can point `hub_url` at a fork:

```
https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json
```
