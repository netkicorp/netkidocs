# Callback Best Practices

Netki delivers a callback to your endpoint whenever a transaction reaches a
state you should react to. This page covers how to consume those callbacks
reliably. For how callbacks are configured and delivered — the payload shape,
retry behavior, and the management endpoints — see [Callbacks](./callbacks2.md).

Common use cases:

- Automating responses to the end user
- Notifying compliance officers of new records
- Triggering token or payment distribution
- Alerting the end user of an approval or denial
- Updating your internal systems
- Kicking off an internal background check

## Handling callbacks

- **Don't block.** Accept the callback, persist the payload, and hand it to a
  background job for processing. Return quickly — if your handler is slow, the
  delivery can time out, which counts as a failed attempt and triggers retries
  (see [Retry and backoff](./callbacks2.md#retry-and-backoff)). Persisting the
  raw payload first also lets you re-process it if your own processing fails.

- **Return the expected status.** A delivery only counts as successful if your
  endpoint returns the exact status configured in `callback_expected_status`
  (default `200`; you may set another `2xx` such as `201 Created` or
  `204 No Content`). Configure it on your callback settings — see
  [Configure your callback](./callbacks2.md#configure-your-callback). Anything
  else is treated as a failed attempt.

- **Authenticate the callback.** Configure basic authentication (recommended)
  or no-auth on your callback settings, and consider allowlisting Netki's
  delivery IP addresses. See
  [Configure your callback](./callbacks2.md#configure-your-callback).

- **Expect duplicates; be idempotent.** You may receive more than one callback
  for the same transaction — from retries, a manual re-send, or a restart. Key
  your processing off the transaction `id` so a repeat delivery doesn't create
  duplicate work.

- **Store the data durably.** Persist the full payload so you can re-process it
  if something goes wrong. A store that understands JSON natively (PostgreSQL,
  MySQL, MongoDB) is easier to work with than flat files. Replicate and back up
  your data — do not rely on Netki as your system of record.

- **Mind retention.** Netki purges records according to its retention policy,
  and timing varies by client, so pull and store what you need promptly.

## Pull document images promptly

Document image links in the payload are time-limited and **will expire**, so
start an asynchronous job to download them as soon as the callback arrives.

A transaction may have several documents (sometimes six or more), and images
may arrive rotated — customers often photograph IDs sideways or upside down.
Netki rotates them during processing for face matching, so what you receive
should be correctly oriented. The images live on each entry of
`transaction_identity.identity_documents` — see
[identity_documents fields](./polling.md#identity_documents-fields) in
[Transactions](./polling.md).

## Understand why a state changed

To learn *why* a transaction is on hold or failed, read the `errors` arrays
rather than any single summary field. An error attached at the transaction or
identity level carries an `error_code` you can act on:

```json
errors[].error_code
```

See [API Error Codes](./api_error_codes.md) for the registry, and
[errors fields](./polling.md#errors-fields) in [Transactions](./polling.md) for
where these arrays appear in the payload.

## Recording manual actions

When your team approves, declines, or restarts a transaction through the
manual dashboard actions, send a `notes` value on the request describing why —
for example an internal case or reviewer reference like `SS1249` — so the
action is traceable later. See
[Mark a transaction as completed](./polling.md#mark-a-transaction-as-completed),
[failed](./polling.md#mark-a-transaction-as-failed), and
[Restart a transaction](./polling.md#restart-a-transaction).

> [!NOTE]
> Be careful when reviewers copy and paste into the `notes` value — stray
> characters from Word or other rich-text sources can cause problems. Prefer
> plain text.

## Transaction states and the payload

For the full transaction shape, the list of states, and field-by-field detail,
see [Transactions](./polling.md).
