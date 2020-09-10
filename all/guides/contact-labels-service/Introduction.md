SortOrder: 0
# Contact Labels Service

## Introduction

Labels service enables to create segmentations in your contacts list.
These segmentations are extremely useful when you want to target a specific audience and reach out via email campaigns. 
Also, when performing bulk actions (such as label & delete contacts), 
you can easily choose the required contacts by using labels to divide them to dedicated categories

### The Contact Label Entity

A contact label is way to group contact. There is a set of built-in (system/predefined) labels (see this [article](https://support.wix.com/en/article/predefined-contact-labels) for details), and there is an option to create new custom (user-defined) label.

A custom label  can be created by the owner in the contact dashboard (see [here](https://support.wix.com/en/article/creating-contact-labels) for details), or via this API.


Contact Label has the following properties:
- `key` - a read-only property that serves as a unique identifier of each contact label.
  - The key is a friendly id, auto-generated based on the first displayName of the label
  - User defined labels will be prefixed with "custom."
- `displayName` - label name - as user sees it. E.g. `Customers`, `Frequent Flyers`, etc. Read-only for SYSTEM labels.
- `labelType` - a read-only property that defines label's type, can be one of the following values:
  - `SYSTEM` - System predefined
  - `USER_DEFINED` - Defined by the user, i.e. custom labels.
  - `WIX_APP_DEFINED` - Defined by a Wix App, i.e. Wix Events labels. (App defined labels caan only be managed by the owning App) 
- `namespace` - Defines the namespace for the label's `key`. (Always `custom` for USER_DEFINED labels).
- `createdDate` - When the label was created.
- `updatedDate` - When the label was last updated.

  
### Example

```json
   {
      "namespace" : "custom",
      "key": "custom.my-label",
      "displayName": "My Label",
      "labelType": "USER_DEFINED",
      "createdDate": "2020-04-20T14:02:20Z",
      "updatedDate": "2020-04-20T14:02:20Z"
   }
```

### Sample Usage
In a *Contact* object:

```json
{
  "id": "8046df3c-7575-4098-a5ab-c91ad8f33c47",
  ...
  "info": {
    ...
    "labelKeys": [
      "custom.my-label"
    ],
   ...
  }
}
```