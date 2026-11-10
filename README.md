# n8n-nodes-facturasdk

This is an n8n community node that lets you use **Factura SDK** in your n8n workflows.

[Factura SDK](https://afipsdk.com/docs/facturasdk/) issues electronic invoices in Argentina end to end: issuers, certificates, authorization in ARCA (formerly AFIP), PDF and webhooks. It is the invoicing product of [Afip SDK](https://afipsdk.com/).

[n8n](https://n8n.io/) is a [fair-code licensed](https://docs.n8n.io/reference/license/) workflow automation platform.

[Installation](#installation)
[Operations](#operations)
[Credentials](#credentials)
[Compatibility](#compatibility)
[Resources](#resources)

## Installation

Follow the [installation guide](https://docs.n8n.io/integrations/community-nodes/installation/) in the n8n community nodes documentation.

Search for `n8n-nodes-facturasdk` in the community nodes panel.

## Operations

### Factura SDK node

| Resource | Operations |
|----------|------------|
| **Invoice** | Create (with `Idempotency-Key` and an optional wait for the final status), Get, Get Many, Cancel |
| **Issuer** | Create, Get, Get Many, Update, Delete, Lookup (registration data of a CUIT), Get Sales Points, Create Sales Point, Create Certificate, Accept Delegation, Verify |
| **Certificate** | Create (upload or create in ARCA), Get, Get Many, Revoke, Renew (upload or in ARCA, keeping its ID), Delete, Authorize |
| **Webhook Endpoint** | Create, Get, Get Many, Update, Rotate Secret, Test, Get Deliveries |
| **Automation** | Get, with an optional wait until it finishes |

The operations that start an automation in ARCA (sales points, certificates created with the tax password, delegation, verification) wait for its result by default; turn off **Wait For Result** to get the automation ID and check it later. Bodies are sent as JSON, exactly as in the [documentation](https://afipsdk.com/docs/facturasdk/invoices). API errors come with the message, the rules that were not met and the problems by field.

### Factura SDK Trigger node

Starts the workflow when Factura SDK sends the result of an invoice: `invoice.approved`, `invoice.rejected` or `invoice.failed` (and `test`, if you select it). When you activate the workflow, the node registers its URL as the webhook endpoint of the environment of the credential and keeps the secret; when you deactivate it, the endpoint becomes inactive. The node verifies the `X-FacturaSDK-Signature` of every delivery and rejects the invalid ones.

Factura SDK accepts one active endpoint per environment. If another endpoint is active, the activation fails and tells you its URL: change that URL to the URL of the node, or deactivate it, and activate the workflow again. To test the trigger in the editor while the workflow is active, use a credential with a sandbox token: the sandbox has its own endpoint. If you prefer to create the endpoint yourself (from the dashboard, *Configuración → Webhooks*, or with the Factura SDK node) with the production URL of the node, save its secret in the credential; the node uses it when it finds the endpoint already registered.

## Credentials

Both nodes use the **Factura SDK API** credential (`facturaSdkApi`):
1. In [app.afipsdk.com](https://app.afipsdk.com/), create a project of type Factura SDK and, in *Configuración → Access tokens*, a token of the environment you want to use: `fsk_dev_…` works in the sandbox, `fsk_prod_…` issues real invoices.
2. In n8n, go to **Settings → Credentials → New** and search for *Factura SDK*.
3. Enter the access token. The webhook secret is only needed if you create the webhook endpoint yourself instead of letting the trigger register it.

## Compatibility

- Tested with n8n `1.x`.
- Requires n8n Nodes API version 1.

## Resources

- [n8n community nodes documentation](https://docs.n8n.io/integrations/community-nodes/)
- [Factura SDK documentation](https://afipsdk.com/docs/facturasdk/)
- [Afip SDK](https://afipsdk.com/)
