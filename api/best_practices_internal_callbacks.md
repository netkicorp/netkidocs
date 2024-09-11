# Best Practices for Internal Callbacks

Netki provides a callback service to notify the client when the transaction state changes. Some possible use cases include:

- Automating responses to the end user
- Notifying compliance officers of new records
- Triggering payment of tokens
- Alerting the end user of approval or denial
- Forcing an update of local systems
- Starting an internal background check

## Callback Implementation Guidelines

When working with callbacks, follow these best practices:

- **Callbacks should NOT block:** Store the data received in the callback and submit it to a background job for processing. This ensures you retain the data for re-processing if there's an issue. It also ensures that if your custom processing takes too long, our system won't time out and stop sending callbacks.
  
- **Return proper status codes:** Use either `200` or another `2xx` status code (`201 Created`, `204 Updated`, etc.). Let us know which status code you’ll use so we can configure it accordingly.
  
- **Authentication:** We allow both basic authentication and no-auth methods. We recommended that you use basic auth and also whitelist our IPs.
  
- **Image retrieval:** Start an asynchronous job to pull in the images for the documents. These links expire, so ensure you have a plan to download them.

- **Record retention policy:** We purge records when appropriate. Some clients retain records longer, while others purge as soon as the transaction is completed. Ensure you store your data with replication and backups.

- **Data storage:** If possible, store the data in a semi-structured or unstructured data store. While storing in text files is acceptable, a better solution would be using a database that understands JSON natively, such as MongoDB, MySQL, or PostgreSQL.

---

### Document Images

Pull in the documents as soon as possible. More than one document may be associated with a transaction, and there could be more than six in some cases. It is not uncommon for some images to be stored sideways or upside down, as end-users often take pictures in such orientations. We rotate images programmatically to detect and match faces. Image rotation is part of several post-processing algorithms.

The document field on the identity document object contains the necessary data:

```json
transaction_identity.identity_documents[]
```

---

### Determining Status Causes

The transaction can be modified in various ways. The best way to understand why something has changed is to gather context from other fields, particularly the `errors` field.

For example, the document below contains an error object at the root level of the JSON:

```json
errors[].error_code
```

If there is an error code attached, it means there was an issue, and the transaction was either on hold or declined by our system.

---

### Identifying Manual Updates

It’s recommended to use a specific key in the "notes" field when updating a transaction. Many clients use this field to indicate why they are performing an action.

> **Note:** Be cautious when allowing clerks, admins, or compliance workers to copy/paste into the notes field. Bad characters can cause issues, often coming from text copied from Microsoft Word or other rich text documents.

The `notes` object is found at the root of the transaction object. Below is an example where `SS1249` was saved into the note field to identify the user who updated the records.

---

### Sample States

For more detailed information on transactions, their states, and samples please go [here](/polling.md).