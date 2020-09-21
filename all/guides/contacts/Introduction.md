SortOrder: 0
# About Contacts
 
The Wix Contact List allows site owners to manage information about site visitors and members.
When a user is captured as a contact,
their details will be available through the contacts APIs.
Here are some examples for how a visitor could be converted to a contact:

- A site visitor fills in a form with their communication details
- A site visitor signs up as a member of the site
- A site contributor
  [imports a contact](https://support.wix.com/en/article/importing-contacts-by-uploading-a-csv-file-1066522) or
  [adds a contact manually](https://support.wix.com/en/article/manually-adding-contacts)
- A third party app adds a contact via the Contacts API

Read more about how site owners can [manage their contact list](https://support.wix.com/en/article/about-your-contact-list).

## Terminology

- [Extended fields](/wix-docs/rest/crm/contacts/extended-fields)
  can be added to the Contact List by site contributors or apps.
  Extended fields store information beyond the default fields,
  and apps can create and manage their own extended fields.
- [Contact labels](/wix-docs/rest/crm/contacts/contact-labels)
  help contributors segment and organize their site’s contacts.
  Contact labels can be system-defined or user-defined,
  and the contact labels for a site are available through the Contact Labels APIs.

## Sample Use Cases and Workflows

Below are some sample use cases for site owners, as well as some API flows to support them.

### Copying New Wix Contacts to an External CRM

Businesses with a separate CRM can copy new Wix contacts
and to their CRM as new leads.

To do this, your app can periodically bulk load contacts
by following this basic flow:

1. For the first request after the app is installed,
  use **List Contacts** to get all contacts. Skip to step 3.
2. For all subsequent requests, use **Query Contacts**,
  and filter for contacts created after the last copy:

    ```json
    {
      "sort": {
        "fieldName": "createdDate",
        "order": "ASC"
      },
      "filter": {
        "createdDate": {
            "$gt": "<PREVIOUS_COPY_DATETIME>"
        }
      }
    }
    ```

3. Upload the returned contacts using the external CRM API.
4. Store the current datetime, and use it in the next request.
  Each subsequent request will need to use the datetime of the request that came before it.

You can allow the site admin to configure the time interval in your app
to fit their needs.

### Two-Way Sync with an External System

Business owners can keep their external CRM or email marketing tool
up to date with Wix with a two-way sync.
To synchronize Wix and an external system,
you’ll need to think about how your app will handle these flows:

- [Initial setup](#initial-setup)
- [Synchronizing from Wix to the external system](#synchronizing-from-wix-to-the-external-system)
- [Synchronizing from the external system to Wix](#synchronizing-from-the-external-system-to-wix)

#### Initial Setup

1. To ensure a 1:1 mapping for each contact,
  use the Extended Fields API to create a field for the external CRM ID.
  To do this, create a request with the **Find or Create Extended Field** endpoint:

    ```json
    {
      "displayName": "externalCrmId",
      "dataType": "TEXT",
    }
    ```

2. The first time a contact is synchronized,
  populate the new field with the ID from the external CRM.
  Craft your code so this ID won’t be overwritten by future updates from your app.
3. Create a mapping from the Wix fields to the external CRM fields,
  to be used whenever Wix and the external CRM are synchronized.
  Store this mapping on your app's server.
4. Handle update loops so that a sync between Wix and the external CRM
  doesn't endlessly trigger synchronizations between the two systems.

#### Synchronizing From Wix to the External System

Listen for any changes to Wix contacts with these webhooks:

- **Contact Created Webhook**
- **Contact Deleted Webhook**
- **Contact Updated Webhook**
- **Contacts Merged Webhook**

When one of these webhooks fires,
your endpoint will receive a request with the contact in the payload.
You can bring those changes to the external CRM with a flow like this one:

1. Get the `slug` (which tells you what the event was)
  and `entityId` (which is the contact ID) from the webhook data.
2. Parse the data object for any other relevant information.
3. Use the mapping (from the [initial setup](#initial-setup))
  to copy the relevant information to the external CRM.
4. Ignore the resulting event from the external CRM
  so you don’t create an endless loop of updates.

#### Synchronizing from the External System to Wix

Listen for changes to contacts using the external CRM APIs.
When your app detects a change that it needs to synchronize,
follow this basic flow:

1. Copy the relevant details using one of these CRUD endpoints.
  Use the endpoint that matches the event from the external CRM:
    - **Create Contact**
    - **Delete Contact**
    - **Update Contact**
    - **Merge Contacts**
    - **Label Contact**
    - **Unlabel Contact**
2. Ignore the resulting event from Wix
  so you don’t create an endless loop of updates.

### New Contacts from an External Form

To create contacts using a form hosted in an external app,
capture the relevant form values, and follow this basic flow:

1. Use the **Find Duplicate Contact** endpoint
  to find out if the contact already exists.
2. If the contact already exists,
  update the contact with the **Update Contact** endpoint.
3. If the contact doesn’t exist,
  create a new contact with the **Create Contact** endpoint.
