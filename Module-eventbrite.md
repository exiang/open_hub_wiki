Eventbrite module not only allows you to sync your events from eventbrite, but also others actor's events into OpenHub. Data such as events, registration, attendance are sync and handle in this module.

## Webhook
### Step 1: Acquire API Key from Eventbrite
1. Login Eventbrite, goto `Account Settings -> Developer Links -> API Keys`
<img width="640" alt="Screenshot 2020-08-24 at 10 40 26 AM" src="https://user-images.githubusercontent.com/5336690/90999163-7a04b380-e5f8-11ea-8489-0058c2ae1c31.png">

2. Click Create API Keys button
3. Fill up details:

   * Application URL: https://openhub.mymagic.my
   * Application Name: OpenHub
   * Application Description: Webhook between my eventbrite account and OpenHub

<img width="653" alt="Screenshot 2020-08-24 at 10 57 50 AM" src="https://user-images.githubusercontent.com/5336690/90999240-ad474280-e5f8-11ea-971c-67f03687c189.png">

4. Click Create Key
5. Click 'Show API key, client secret and tokens' on this created API
6. Copy the Private token, this will be your Eventbrite OAUTH Secret

### Step 2: Setup Webhooks in Eventbrite
1. Login Eventbrite, goto `Account Settings -> Developer Links -> Webhooks`
2. You will need to create 2 webhooks here that this OpenHub eventbrite module supported:

<img width="640" alt="Screenshot 2020-08-24 at 10 48 44 AM" src="https://user-images.githubusercontent.com/5336690/90999611-b258c180-e5f9-11ea-9165-fb0a6b27d24a.png">

#### when event created & updated
* Payload URL: Insert `https://openhub.mymagic.my/eventbrite/callback/eventChanges`
* Event: Select `All Event`
* make sure you checked `event.created`, `event.updated`, `event.published`, `event.unpublished`, `organizer.updated`, `venue.updated` too

#### when registration and attendance created & updated
* Payload URL: Insert `https://openhub.mymagic.my/eventbrite/callback/attendeeChanges`
* Event: Select `All Event`
* make sure you checked `attendee.updated`, `attendee.checked_in`(`barcode.checked_in`), `attendee.checked_out`(`barcode.un_checked_in`) too

### Step 3: Setup Webhooks in OpenHub
You will be linking your eventbrite account with your organization record in OpenHub. Once done, event created/updated as well as registration made, attendance taken will be automatically sync to your OpenHub using a method called web callback. OpenHub will auto set the correct organization owner and role base this registry.

1. Go to OpenHub backend: `Event -> Sync from Eventbrite`


<img width="640" alt="Screenshot 2020-08-24 at 10 41 49 AM" src="https://user-images.githubusercontent.com/5336690/90999311-dec00e00-e5f8-11ea-8071-e8b044985b43.png">

2. Click `Go` button to register a new webhook

<img width="640" alt="Screenshot 2020-08-24 at 10 41 59 AM" src="https://user-images.githubusercontent.com/5336690/90999390-057e4480-e5f9-11ea-8f36-ec943d8ac570.png">

3. Insert Organization Code (an UUID which looks something like this `bd1df76d-5490-4562-a8ca-ed1a8cb3f422`)
4. Insert the `Private Token` you copied in `Step 1` into `Eventbrite OAUTH Secret`
5. You will need your eventbrite Account Id. You can acquired it from eventbrite: 
   1. Click `Test` button one of the created webhooks in eventbrite
   2. Wait for a minute
   3. Once callback is done, don't worry about its fail error, expands the section for detail. You can then extract the `user_id` out. e.g. In this case, 107038522227
```
Request Payload
{
  "api_url": "https://www.eventbriteapi.com/v3/events/101621827622/",
  "config": {
    "action": "event.updated",
    "endpoint_url": "https://central.mymagic.my/eventbrite/callback/eventUpdate",
    "user_id": "107038522227",
    "webhook_id": "1750814"
  }
} 
```

<img width="640" alt="Screenshot 2020-08-24 at 10 55 21 AM" src="https://user-images.githubusercontent.com/5336690/90999794-2b581900-e5fa-11ea-9273-25b6050f996f.png">

6. If you setup everything correctly, at OpenHub Backend `Event -> Sync from Eventbrite`, you should see your organizations display in the list. Click on it and you should see the list of events available in your eventbrite account.


