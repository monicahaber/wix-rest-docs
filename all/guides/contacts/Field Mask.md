SortOrder: 2
# FieldMask

in `UpdateContact` endpoint you are asked to provide `fieldMask` which represents the required 
fields of the contact to update.

## Valid Contact Field Mask Fields

The following fields can be updated by specifying them in the `fieldMask` array:

| Field |
|---|
| `name` |
| `emails` |
| `phones` |
| `addresses` |
| `company` |
| `jobTitle` |
| `birthdate` |
| `assignedUserIds`|
| `locale` |
| `labelKeys` |
| `extendedFields` |
| `extendedFields.invoices.vatId` |
| `extendedFields.members.membershipStatus` |
| `extendedFields.members.mobile` |
