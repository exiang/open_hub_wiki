## Sync to Event
One of the benefit of using F7 form is it has built in ability to sync form submissions to event. Both Organization and Individual registration information can be extract from a F7 Form and sync to Event as 'Company Participants' (`event_organization`) & 'Registrations' (`event_registration`).

First,  `Event Mapping Instruction` in the form must be set. This will tell the system on how to map data between a F7 form and event.

```json
{
    "event_id": 63652,
    "is_sync_draft": true,
    "is_sync_organization": true,
    "organizations": [
        {
            "name": "Participated Startup",
            "map2modal": "startup",
            "workflows": {
                "application": "participant",
                "screening": "participant",
                "inprogram": "selectedParticipant",
                "rejected": "rejectedParticipant"
            }
        }
    ],
    "is_sync_event_registration": true,
    "event_registrations": [
        {
            "name": "Founder 1",
            "attendance_map_workflow": [
                "inprogram"
            ],
            "mappings": {
                "full_name": "f7.f1name",
                "email": "f7.f1email",
                "phone": "f7.f1phone",
                "gender": "f7.f1gender",
                "organization": "f7.startup",
                "nationality": "MY"
            }
        }
    ]
}
```

Please note that on each execution, system will automatically clear all target data with the same form and vendor code `f7`.

## Problems
This module was heavily hard coded by previous developer and lots of refactor and rework needed to set it right. Here are few issues to take notes:
  - File upload is hardcoded to 10MB only
  - Survey is hardcoded to the database table
  - views generated are buggy