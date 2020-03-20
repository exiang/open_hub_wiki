## Sync to Event
One of the benefit of using F7 form is it has built in ability to sync form submissions to event. Both Organization and Individual registration can be sync.

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
                "inProgram": "selectedParticipant",
                "rejected": "rejectedParticipant"
            }
        }
    ],
    "is_sync_event_registration": true,
    "event_registrations": [
        {
            "name": "Founder 1",
            "attendance_map_workflow": [
                "approved"
            ],
            "mappings": {
                "eventRegistration.fullname": "f7.f1name",
                "eventRegistration.email": "f7.f1email",
                "eventRegistration.phone": "f7.f1phone",
                "eventRegistration.gender": "f7.f1gender",
                "eventRegistration.nationality": ""
            }
        }
    ]
}
```

Please note that on each execution, system will automatically clear all target data with the same form and vendor code `f7`.