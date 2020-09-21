SortOrder: 3
# About Domain Events

As of V4 Contacts Service we started to use Domain Events. 
Those events replace the previous events sent by contacts service and you are recommended to listen to those event instead of the older ones.

The existing domain events are:

 - Contact Created Domain Event 
 - Contact Updated Domain Event
 - Contact Deleted Domain Event
 - Contacts Merged Domain Event

you can read about each of those events in a dedicated section.
the topic of those new events is: `domain_events_wix.crm.contacts.contact`
the `entityId` field on the event body is the `contact id` and for each type of event there is a field for the slug (String) and a info of the event:

-  Contact Created: slug is `created` and the info is on the field `createdEvent`
-  Contact Updated: slug is `updated` and the info is on the field `updatedEvent`
-  Contact Deleted: slug is `deleted` and the info is on the field `deletedEvent` (which is empty)
-  Contact Merged: slug is `merged` and the info is on the field `actionEvent`

for each contact domain event there are also `eventTime` field which is a string with the event timestamp,
a boolean `triggeredByAnonymizeRequest` - whether this event was triggered as a result of a privacy regulation application
and `entityFqdn` which equals `wix.crm.contacts.contact`

# Moving from legacy events to domain events
The transition is simple,

The `Created, Updated, Deleted, Merged` events are also exist at V1 service (legacy code) so you should listen to the new domain events instead.
the topic name for those events: `crm-contacts-events`

you can view the v1 events here:
https://github.com/wix-private/crm/blob/master/contacts/core/contacts-api/src/main/proto/v1/webhooks/contact_events.proto

The `Created, Updated, Deleted` events are also exist on the legacy bootstrap projects (also, legacy code), so if you listen to those events you are also 
advised to listen instead to the domain events.
the topic names for those events: `contacts-changed-events` and `contacts-update`

you van view the bootstrap events here:
https://github.com/wix-private/crm/blob/master/wix-contacts-server/wix-contacts-server-api/src/main/scala/com/wixpress/contacts/api/greyhound/ContactChangedEvent.scala
the `notificationAction` holds the type of the event (`Created/Updated/Deleted`)

and also here:
https://github.com/wix-private/crm/blob/master/wix-contacts-server/wix-contacts-server-core/src/main/scala/com/wixpress/contacts/handlers/ContactDetailsNotificationHandler.scala
