SortOrder: 6
# Filter and Sort

_List_ and _Query_ endpoints allow sorting results by field. Use `field:asc` to sort results in **ascending** order, and `field:desc` to sort in **descending** order.

For example, to sort contacts by First Name in descending order: 

```
?sort=info.name.first:desc
```

or 

```json
"sort": "info.name.first:desc"
```
The default sort is `createdAt:asc`

Refer to the following tables to check which fields support sorting.

## List & Query Contacts

| Field                                                        | Description                           | Query Filter Operators                            | Possible Values                                       |Sorting | 
|--------------------------------------------------------------|---------------------------------------|---------------------------------------------------|-------------------------------------------------------|--------|
| id                                                           | Contact unique Id                     | $eq,$ne,$in                                       | Guid                                                  |        |
| createdDate                                                  | Creation date                         | $eq, $neq, $gt, $lt, $gte, $lte                   | Date in UTC format                                    |Allowed |
| updatedDate                                                  | Last update date                      | $eq, $neq, $gt, $lt, $gte, $lte                   | Date in UTC format                                    |        |
| lastActivity.activityDate                                    | Date of last contact activity         | $eq, $neq, $gt, $lt, $gte, $lte                   | Date in UTC format                                    |Allowed |
| primaryInfo.email                                            | Contact primary email                 | $eq, $neq, $in, $startsWith, $endsWith, $contains | Valid Email String                                    |Allowed |
| primaryInfo.phone                                            | Contact primary phone                 | $eq, $neq, $in, $startsWith, $endsWith, $contains | Valid Phone String                                    |
| info.name.first                                              | Contact first name                    | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |Allowed |
| info.name.last                                               | Contact last name                     | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |Allowed |
| info.emails.email                                            | Contact email                         | $eq, $neq, $in, $startsWith, $endsWith, $contains | Valid Email String                                    |Allowed |
| info.phones.phone                                            | Contact phone                         | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |        |
| info.addresses.street                                        | Contact address (Street)              | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |        |
| info.addresses.city                                          | Contact address (city)                | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |        |    
| info.addresses.subdivision                                   | Contact address (State/Region)        | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |        |     
| info.addresses.country                                       | Contact Country                       | $eq, $neq, $in, $startsWith, $endsWith, $contains | Country ISO 2 letter code                             |        |
| info.company                                                 | Contact company                       | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |Allowed |
| info.jobTitle                                                | Contact job title                     | $eq, $neq, $in, $startsWith, $endsWith, $contains | String                                                |Allowed |
| info.birthdate                                               | Contact date of birth                 | $eq, $neq, $gt, $lt, $gte, $lte                   | Date in UTC format                                    |Allowed |
| info.locale                                                  | Contact language                      | $eq,$ne,$in                                       | Locale 2 letter code                                  |        |
| info.labelKeys                                               | Labels associated with the Contact    | $in                                               | String                                                |        |
| info.extendedFields.invoices.vatId                           |                                       | $exists                                           |                                                       |        |
| info.extendedFields.members.membershipStatus                 | Contact Member status                 | $eq,$ne,$in                                       | `APPROVED`, `DENIED`, `PENDING` or `INACTIVE`         |        |
| info.extendedFields.members.mobile                           | Is Contact a member mobile            | $eq,$ne                                           | `true`/`false` as String                              |        |
| info.extendedFields.ecom.numOfPurchases                      | Total purchases count made by Contact | $eq, $neq, $gt, $lt, $gte, $lte                   | Int                                                   |Allowed |
| info.extendedFields.ecom.totalSpentAmount                    | Total money spent by Contact          | $eq, $neq, $gt, $lt, $gte, $lte                   | Double                                                |Allowed |
| info.extendedFields.emailSubscriptions.subscriptionStatus    | Contact subscription status           | $eq,$ne,$in                                       | `SUBSCRIBED`, `UNSUBSCRIBED`, `NOT_SET` or `PENDING`  |        |
| info.extendedFields.emailSubscriptions.deliveryStatus        | Contact email delivery status         | $eq,$ne,$in                                       | `VALID`, `BOUNCE`, `INVALID`, `INACTIVE`              |        |
| info.extendedFields.emailSubscriptions.effectiveEmail        | Contact effectiveEmil                 | $exists                                           |                                                       |        |
| info.extendedFields.custom.(fieldKey)                        | Site-owner-define Contact field(s)    | According to field data type                      | String/Number/Date                                    |Allowed |

### Querying User defined fields

|Extended Field Data Type | Query Filter Operators                              |
|-------------------------|-----------------------------------------------------|
| String                  | $eq, $neq, $in, $startsWith, $endsWith, $contains   |
| Number                  | $eq, $neq, $gt, $lt, $gte, $lte                     |
| Date (UTC format)       | $eq, $neq, $gt, $lt, $gte, $lte                     |

## Examples

**Get contacts named 'John'**

```
curl 'https://www.wixapis.com/contacts/v4/contacts/query' --data-binary '{"query":{"filter":"{\"info.name.first\": \"John\"}"}}' -H 'Content-Type: application/json' -H 'Authorization: XXX'
``` 

**Get contacts with last name 'Smith', sorted by date of creation**

```
curl 'https://www.wixapis.com/contacts/v4/contacts/query' --data-binary '{"query":{"filter":"{\"info.name.last\": \"Smith\"}, {"sort":"[{\"createdAt\": \"asc\"}]"}}' -H 'Content-Type: application/json' -H 'Authorization: XXX'
``` 

**Get contacts by IDs**

```
curl 'https://www.wixapis.com/contacts/v4/contacts/query' --data-binary '{"query":{"filter":"{\"id\": {\"$in\": [\"CONTACT_ID_1\",\"CONTACT_ID_2\"]}}"}}' -H 'Content-Type: application/json' -H 'Authorization: xxx'
```