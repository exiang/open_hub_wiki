Eventbrite module not only allows you to sync your events from eventbrite, but also others actor's events into OpenHub. Data such as events, registration, attendance are sync and handle in this module.

## Webhook
### Acquire API Key from Eventbrite
1. Login Eventbrite, goto `Account Settings -> Developer Links -> API Keys`
2. Click Create API Keys button
3. Fill up details:
  * Application URL: https://openhub.mymagic.my
  * Application Name: OpenHub
  * Application Description: Webhook between my eventbrite account and OpenHub
4. Click Create Key
5. Click 'Show API key, client secret and tokens' on this created API
6. Copy the Private token, this will be your Eventbrite OAUTH Secret

### Setup Webhooks in Eventbrite
1. Login Eventbrite, goto `Account Settings -> Developer Links -> Webhooks`
2. You will need to create 3 webhooks here that this OpenHub eventbrite module supported:

#### when event created
  * Payload URL: Insert `https://openhub.mymagic.my/eventbrite/callback/eventCreate`
  * Event: Select `All Event`
  * make sure you checked `event.created` too

#### when event updated
  * Payload URL: Insert `https://openhub.mymagic.my/eventbrite/callback/eventUpdate`
  * Event: Select `All Event`
  * make sure you checked `event.updated` too

#### when registration and attendance created & updated
  * Payload URL: Insert `https://openhub.mymagic.my/eventbrite/callback/attendeeChanges`
  * Event: Select `All Event`
  * make sure you checked `barcode.checked_in`, `attendee.updated`, `barcode.un_checked_in` too

### Setup Webhooks in OpenHub