# Google Contacts MCP server

Inspect personal contacts and create a reviewed contact. Read provider data first, then explicitly approve the selected change with operator write opt-in and a unique per-call approval key.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `GOOGLE_CONTACTS_ACCESS_TOKEN`



Credential help: https://developers.google.com/people/api/rest/v1/people.connections/list

## Supported scope

- personId is the suffix returned in resourceName people/c123, not the whole slash-separated resource name. Create-contact requests should run sequentially for one user.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call list_connections '{"personFields":"names,emailAddresses","pageSize":10}'
```

## Use with an MCP client

Launch `node /absolute/path/google-contacts-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| list_connections | Read | List connections. |
| get_contact | Read | Get contact. |
| create_contact | Explicit approval required | Create contact. |

## Concrete workflow

Inspect personal contacts and create a reviewed contact. Read provider data first, then explicitly approve the selected change with operator write opt-in and a unique per-call approval key.

1. list_connections
2. get_contact
3. create_contact

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Activate with PipMCP](https://pipmcp.com/github/start?app=google-contacts&integration=google-contacts-mcp&utm_source=github&utm_campaign=github-google-contacts-mcp&utm_content=readme) when you want the hosted service.  Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/google-contacts/).

## Provenance and maintenance

- https://developers.google.com/people/api/rest/v1/people.connections/list
- https://developers.google.com/people/api/rest/v1/people/get
- https://developers.google.com/people/api/rest/v1/people/createContact

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
