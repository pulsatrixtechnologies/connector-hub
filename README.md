# Pulsatrix connector hub

A public list of products the Pulsatrix connector can add: OpenAPI, Swagger, RPC and similar API documents, compiled to catalogs.

The console starts empty. An operator opens Products, picks a card (ConnectWise Platform, SentinelOne, AlertOps, ...) and taps Add. Profiles then grant that product to people.

## This repository

| File | What it is |
|---|---|
| `index.json` | `pulsatrix-hub/1`: name, summary, tags, default catalogs |
| `.github/ISSUE_TEMPLATE/add-product.md` | how to ask for a missing vendor |

The compiled catalogs themselves live in the connector repo (`catalog/`). The hub is the index the console shows, not a second copy of every document.

## Tags

- `rest` REST API
- `openapi` OpenAPI
- `swagger` Swagger
- `graphql` GraphQL
- `rpc` RPC methods
- `account` needs a login or a vendor document the operator already has
- `private` private API (the vendor's own site, with the client's authorisation)

## Request a product

Open an issue with the Add a product template. Include the vendor name, a public document URL if there is one, and whether a login is required. Do not attach keys, tokens or tenant hostnames.

## Use it from a connector

The gateway embeds this index. An operator can point `hub_url` at a fork:

```
https://raw.githubusercontent.com/pulsatrixtechnologies/connector-hub/main/index.json
```
