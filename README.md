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

## Tags

- `rest` REST API
- `openapi` OpenAPI
- `swagger` Swagger
- `graphql` GraphQL
- `rpc` RPC methods
- `account` needs a login or a vendor document the operator already has
- `private` private API (the vendor's own site, with the client's authorisation)

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

Notes on public files:

- Datto RMM platforms share one schema. Pinotage answered 200. Merlot, Vidal, Concord and Zinfandel answered 500 on the same path the day this was checked. Swagger UI: `https://{platform}-api.centrastage.net/api/swagger-ui/index.html`.
- HaloPSA is instance-hosted. The Halo Service Desk demo spec is public. A tenant copy also lives at `{instance}/api/swagger/v2/swagger.json`.
- N-central also ships Swagger on the appliance at `{fqdn}/api-explorer`.
- Microsoft Graph v1.0 is about 44 MiB. The connector YAML node budget currently refuses that compile. The hub still lists it so an operator can split or wait on a later compiler.

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
| UniFi, BigLeaf | vendor developer | Account |
| ScalePad, TimeZest, SmileBack, StreamOne, Sherweb, SalesBuildr, PandaDoc, Quote Manager | vendor developer | Account |
| 3CX, Better Stack, Rootly, runZero, HubSpot, QuickBooks Online, Google Workspace | vendor developer | Account or OAuth app |
| Crewhu, Mailprotector, Mimecast, IRONSCALES, SpamTitan, Nutanix, Clio, Kaseya BMS | vendor developer | Account |

Community unofficial specs exist for some gated APIs (Atera YAML in PSAtera, IT Glue YAML at jmaddington/ITG-Glue-OpenAPI, Hudu snapshots in n8n-nodes-hudu). They are not the vendor document. The hub `source` stays on the official page.

## Referenced, no published OpenAPI

MSP products seen in the wild with no OpenAPI, Swagger, GraphQL schema or RPC document found for the hub:

- CIPP (CyberDrain Improved Partner Portal): instance REST, no published spec
- Analytics 365, ClickUp, Warmly, PostHog, Alternative Payments, Slack, Microsoft Teams as collaboration channels

Ask for those with the Add a product template if a document appears.

## Request a product

Open an issue with the Add a product template. Include the vendor name, a public document URL if there is one, and whether a login is required. Do not attach keys, tokens or tenant hostnames.

## Use it from a connector

The gateway embeds this index. An operator can point `hub_url` at a fork:

```
https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json
```
