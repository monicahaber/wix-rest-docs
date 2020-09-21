SortOrder: 4
# Merge Contacts

## Introduction

Nowadays, many site owners are suffering from contacts duplication, these duplications can be caused by different reasons.
Therefore we launched the `Contacts Merge` feature, that allow site owners and contributors to merge duplicate contact.
Any contact that is not member can be merged into another contact


## Merge endpoint

You can find `Merge Contacts` under Contacts Service API [here](https://github.com/wix-private/crm/blob/master/contacts/core/contacts-api/src/main/proto/v4/contacts_service.proto)
- Merge source contacts into target contact
    Endpoint input
    - Target contact id
    - Target contact revision (for updating the contact)
    - An array of source contact ids (up to 5 contacts)
- Contact Info will be merged according the following logic:
    - If the target name is not empty, name will not be merged
    - Each data conflict will be resolved by the following hierarchy : target contact, first source contact, second source contact and so on
    - ALL of the emails, phones, labels, extended fields will be appended
    - Save the source_ids and all it's previous source_its into merged contacts table, pointing to targetContactId
    - Delete source contacts
    - Send ContactsMerged event


## Preview Merge

We also launched a `Preview Merge` endpoint that return the Merge result of a contact target and contact sources.


## Integration

If your vertical keep track of the sites contact ids you should listen to Action Item `MergedContact` on `domain_events_contact` topic.

As part of Merge Contact feature:

- An Action Domain Event will be sent with the following payload:

        {
            contactTargetId : "d8f0d88c-af92-4237-833f-56aaaa2b5168" , 
            sourceContactIds: ["f274f4a0-664a-457a-a83a-d46ea5fb9f54"] 
        }

- Update Domain Event will be sent
- Delete Domain Event won't be sent


## Fallbacks

We will keep supporting the merged contact ids on GetById endpoint.
That means that if `Contact A` was merged to `Contact B`
The result of GetContact(`Contact A Id`) will be `Contact B`
You should be aware that this is the only endpoint that support the merged ids.
That means that QueryBy(`Contact A Id`) will not return `Contact B`


# GDPR consideration

Merged contact ids will be sent as part of UoU Gdpr request.


## Example (with partial contacts):

    
    {
      "id": "8046df3c-7575-4098-a5ab-c91ad8f33c47",
      "revision": 1,
      "primaryInfo": {
        "email": "target@example.com",
        "phone": "097495550"
      },
       "emails": [
          {
            "id": "d8f0d88c-af92-4237-833f-56aaaa2b5168",
            "tag": "MAIN",
            "email": "target@example.com",
            "primary": true
          }
        ],
        "phones": [
          {
            "id": "e5214c79-af80-4eff-87f2-408b0be4e710",
            "tag": "HOME",
            "countryCode": "IL",
            "phone": "097495550",
            "e164Phone": "+97297495550",
            "primary": true
          }
        ],
        "company" : "Wix",
        "labelKeys": [
          "contacts.contacted-me",
          "custom.my-label"
        ],
        "extendedFields": {
          "custom.color": "Blue"
        }
      }
    }
    
    
    
    {
      "id": "f274f4a0-664a-457a-a83a-d46ea5fb9f54",
      "revision": 1,
        "emails": [
          {
            "id": "c7f05j8c-af92-4237-833f-43aaaa2b4968",
            "tag": "MAIN",
            "email": "source@example.com",
            "primary": true
          }
        ],
        "phones": [
          {
            "id": "as465d4c-dht4-4eff-ftr4-408b0be4e710",
            "tag": "HOME",
            "countryCode": "IL",
            "phone": "78457445",
            "e164Phone": "+972978457445",
            "primary": true
          }
        ],
        "jobTitle" : "Developer",
        "labelKeys": [
          "contacts.contacted-me",
          "custom.my-other-label"
        ],
        "extendedFields": {
          "custom.color": "Green",
          "custom.food": "Pizza"
        }
      }
    }

Will be merged with target `'8046df3c-7575-4098-a5ab-c91ad8f33c47'` and sources `['f274f4a0-664a-457a-a83a-d46ea5fb9f54']` to:

        {
          "id": "8046df3c-7575-4098-a5ab-c91ad8f33c47",
          "revision": 1,
          "primaryInfo": {
            "email": "target@example.com",
            "phone": "097495550"
          },
           "emails": [
              {
                "id": "d8f0d88c-af92-4237-833f-56aaaa2b5168",
                "tag": "MAIN",
                "email": "target@example.com",
                "primary": true
              },
            {
              "id": "c7f05j8c-af92-4237-833f-43aaaa2b4968",
              "tag": "MAIN",
              "email": "source@example.com",
              "primary": false
            }
            ],
            "phones": [
              {
                "id": "e5214c79-af80-4eff-87f2-408b0be4e710",
                "tag": "HOME",
                "countryCode": "IL",
                "phone": "097495550",
                "e164Phone": "+97297495550",
                "primary": true
              },
                {
                  "id": "as465d4c-dht4-4eff-ftr4-408b0be4e710",
                  "tag": "HOME",
                  "countryCode": "IL",
                  "phone": "78457445",
                  "e164Phone": "+972978457445",
                  "primary": true
                }
            ],
            "jobTitle" : "Developer",
            "company" : "Wix",
            "labelKeys": [
              "contacts.contacted-me",
              "custom.my-label",
               "custom.my-other-label"
            ],
            "extendedFields": {
              "custom.color": "Blue",
               "custom.food": "Pizza"
            }
          }
        }

        
## Questions and to get help

For any other question
Fell free to contact us on #contacts-merge [slack channel](https://join.slack.com/share/zt-gusm5oja-CThRUWqlow78oqZAIYtNCA)
