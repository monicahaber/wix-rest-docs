SortOrder: 1
# Fieldsets and Projected Fields

Some endpoints allow you to return a partial `contact` object with only the fields you request.
To do this, you can use fieldsets and projected fields.
For common use cases, we provide fieldsets, which are predefined named sets of fields.
If you don’t see a fieldset for your use case,
you can use projected fields, where you specify fields to return.

Fieldsets and field projection are available for these endpoints:

* Get Contact
* List Contacts
* Query Contacts

> **Note:** If you use a combination of fieldsets and projected fields,
> or if you use multiple fieldsets,
> the `contact` object will be returned with all of the requested fields.

## Valid Contact Projection Fields

Use a field projection to return a partial `contact` object with only what you request in the `fields` array.

The following fields can be returned in a partial object by specifying them in the `fields` array:

> **Note:** `id` and `revision` are always returned,
> even if you don’t include them in the `fields` array in your request.

| Field |
|---|
| `id` (always returned) |
| `revision` (always returned) |
| `source` |
| `createdDate` |
| `updatedDate` |
| `lastActivity` |
| `picture` |
| `primaryInfo` |
| `info.name` |
| `info.emails` |
| `info.phones` |
| `info.addresses` |
| `info.company` |
| `info.jobTitle` |
| `info.birthdate` |
| `info.assignedUserIds`|
| `info.locale` |
| `info.labelKeys` |
| `info.extendedFields` |

## Contact Fieldsets

Fieldsets let you return a predefined partial `contact` object.
Fieldsets are built by Wix for common use cases.
You’ll specify fieldsets in the `fieldSet` array in your request.

The following table shows the fields that are returned with each fieldset.
An empty `fieldSet` array returns the `BASIC` fieldset:

| Fieldset | Returned Fields |
|---|---|
| `BASIC` (default) | `id`, `revision`, `info.name.first`, `info.name.last`, `primaryInfo.email`, `primaryInfo.phone` |
| `COMM_DETAILS` | `id`, `revision`, `info.name.first`, `info.name.last`, `info.emails`, `info.phones`, `info.addresses` |
| `EXTENDED` | `id`, `revision`, `info.name.first`, `info.name.last`, `primaryInfo.email`, `primaryInfo.phone`, `info.extendedFields` |
| `FULL` | All fields |
