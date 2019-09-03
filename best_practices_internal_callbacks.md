# Best practices for internal callbacks.

Netki provides a callback service to help notify the end user that the transaction state has been changed.

Some of the possible use cases are:

* automating responses to the end user
* notifying compliance officers of new records
* triggering payment of tokens
* alerting the end user that they have been approved or denied
* forcing an update of local systems
* starting an internal background check


When working with callbacks it is important to follow the suggested implementation policy.

* Callbacks should NOT block. What we mean by this is we request that you do a very superficial check of the data integrity and then store it. Do not force our system to wait or we may experience timeouts. 
* Return proper status codes. Either 200 or 2xx series (created|updated) is acceptable. Let us know what that status code will be so that we can set it.
* We allow basic and no-auth methods. It is recommended to put in basic auth at the webserver level and ALSO whitelist our IP.
* We suggest starting an asynchronous job to pull in the images for the documents.  The links to the images attached to the identity will expire so it is important to have a plan to download them.
* Our policy is to purge records when appropriate. Some people will retain records for longer while others may get purged as soon as the transaction has a complete handoff. Make sure you have your data stored in replication and with backups.
* Put the data into an semi-structured or unstructured data store if possible.  Keeping them in text files is okay, but a better solution would be something that understands JSON data natively such as MongoDB, MySQL or PostgreSQL.



Document Images.

Please pull in the documents as soon as possible.  There will be more than one document associated to a transaction and could be more than 6 in some cases. It is not uncommon to see that we will store some images on their sides or upside down. Routinely the end-user will take a picture upside down or on its side. We will rotate the image programmatically trying to find faces and match them.  Rotating images is part of several post processing algorithms.

The document field on the identity document object will contain the appropriate data.

> transaction_identity.identity_documents[]


Determining Status Causes

The transaction can be modified in many ways. The best way to find out why something has changed is to gather context from some of the other fields.  The best place to find context is to look at the

For an example. The document returned below as an error object at the ROOT level of the JSON:

> errors[].error_code

To keep things simple, if there is an error code attached then there was previously an issue with it and either it was on hold or it was declined by our system.

Identifying manual updates.

It is recommended that you keep a specific key in the "notes" field when you are updating a transaction. Most of our clients use this field to indicate whey they are doing something.

NOTE: Be careful letting your clerks/admins/compliance workers copy/paste into the notes field. We have seen bad characters throw things off. Some of these characters may come from pulling text from Microsoft Word or other rich text documents.

The notes object is found at the root of the transaction object. See below where we have saved SS1249 into the note field to identify the user who updated the records.

Sample States:

Here are the available states that you will see. Any deviation from this will mean something else may be out of alignment and our support staff should be notified.

Pay specific attention as to which state changes are allowed.  It can go to various other states, but it isn't a free flow from any state to any other state. There is an FSM in place controlling this.  Look for CHANGES on the descriptions.



"state": "completed"
"state": "failed"
"state": "post_processing",
"state": "hold",
"state": "restarted",


* failed - same as "declined" on the dashboard
* A callback will show failed for some of the following reasons.
* expired ID
* unable to extract any data
* extracted bad data (invalid characters)
* manually declined
* CHANGES -> restarted
FAILED = 'failed'

* completed - same as "approved" on the dashboard
* an transaction was successfully completed
* also a manual approve action
COMPLETED = 'completed'


* a transaction requires extra processing
* will be managed by an internal team
POST_PROCESSING = 'post_processing'

* the ID did not make a minimum level of quality
* CHANGES -> completed
* CHANGES -> failed
HOLD = 'hold'

* manually restarted
RESTARTED = 'restarted'


{
  "id": "1adaab36-654d-4aaa-a271-d4f7c78e868c",
  "client": "40e23ece-7034-4af1-824a-d4a9cd9e1f7b",
  "transaction_identity": {
    "id": "3066664f-73d6-4579-b3d7-f2b75250ad0c",
...
}
