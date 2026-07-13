# BaaS runtime — payments

## Invoking payments from the runtime GraphQL API
Prerequisites are design-time (payments.md): payment module enabled and bound to the order table,
gateway credentials configured. Create the order SERVER-SIDE first — an actionflow takes product
ids, computes the authoritative amount, inserts the order row, and returns its id; never trust a
client-computed price. Fulfillment happens ONLY in the idempotent payment callback flow — never on
the return/result page. The gateway's webhook is asynchronous: after payment, wait ~2s before the
first order-status check, then poll briefly.

Stripe (PaymentIntent flow — the platform creates the intent server-side; never embed your own
Stripe secret keys in the frontend):
  mutation { stripePayV2(payDetails:{ order_id:…, currency:…, amount:… }) {
      paymentClientSecret stripeReadableAmount } }
payDetails carries the order id, the ISO currency code, and the amount in major units (introspect
for exact input field casing). Confirm on the frontend with Stripe.js Elements using
paymentClientSecret; stripeReadableAmount is a display-ready amount string. stripePay(payDetails)
is the older variant returning the bare clientSecret string. To reconcile after confirmation, query
findOrderIdByPaymentIntentId(paymentIntentId:…). Stripe subscriptions and refunds are modeled in
payments.md — drive them from there.

## Testing against the deployed backend (CLI)

This is a **runtime** spoke — it describes calling a DEPLOYED Momen app's SINGLE auto-generated
GraphQL API, which exposes ALL backend interactions (database, action flows, third-party APIs, AI
agents), not editing the editor schema. Endpoints (`{projectExId}` = the project's external id):
- HTTP (queries + mutations): https://villa.momen.app/zero/{projectExId}/api/graphql-v2
- WebSocket (subscriptions):  wss://villa.momen.app/zero/{projectExId}/api/graphql-subscription

Exercise runtime queries/mutations straight from this CLI — already authenticated with the admin token:

```bash
"${PLUGIN_ROOT:-${CLAUDE_PLUGIN_ROOT}}/bin/momen-mcp" runtime graphql --args '{"query":"query { <root_op> { ... } }","variables":{}}'
"${PLUGIN_ROOT:-${CLAUDE_PLUGIN_ROOT}}/bin/momen-mcp" runtime query   --args '{"tableName":"post","where":{"id":{"_eq":1}},"limit":20,"fields":["id","title"]}'
```
`runtime graphql` sends **raw** GraphQL (use the operator-first `where` grammar in `baas-database.md`); `runtime query/insert/update/delete` are typed helpers that take the **simplified** `where` (see `schema-table.md`). Subscriptions (async action-flow results, AI streaming) run from your generated frontend over the WebSocket endpoint (legacy `subscriptions-transport-ws` framing — see `baas-database.md`) — this CLI does not open runtime subscriptions.

Enable the module, design the order table, and wire the idempotent callback flows via the design-time `payments.md`; create orders server-side with an actionflow (`baas-actionflow.md`) — never compute price on the client.
