F7 is a google form inspired module with aim to allow admin quickly create form without any coding needed. MaGIC is using it for programs like Bootcamp, YouthColab and other small and straightforward programs, with an aim to replace ATAS for accelerator program in near future.

Forms can be group into intake in F7, allowing multiple forms for different process of a same program can display in one interface. 

Related database tables:
* `form`
* `form_submission`
* `form2intake`
* `tag2intake`
* `impact2intake`
* `industry2intake`
* `persona2intake`
* `sdg2intake`
* `startup_stage2intake`


## Intake
Intake is the way to group multiple forms under 1 event into a logical structure. For example, throughout the entire 'MaGIC Accelerator Program 2016', multiple forms involve:
* Application Form (default)
* Pre-arrival Form
* Claim Form
* Giving Back Form
* Post-Program Feedback Form

Hence, you will like to create an intake called 'MaGIC Accelerator Program 2016' and link all the others individual forms to it. 

To reuse a form for other intakes, simply duplicate it. 

> Submission data is linked to the specific form. Since form is not a template, you will need to duplicate to reuse it.

## Form & Submission
Both form structure and submission data in F7 Form is stored as JSON format inside database table cell. This design allow flexiblity in creating all kind of form without hard code into a database table structure, trading off with the simplicity of SQL query. 

## Form Structure
Here is a [Sample F7 Form Structure](Sample-F7-Form-Structure) that demo all the supported form components.

> It's not suggested to change the form structure after it is published and had collected user submissions.
### Section Component
### Group Component
### Label Component
### Headline Component
### Break Component
### Divider Component
### Button Component
```
 {
 	"tag": "button",
 	"prop": {
 		"name": "Submit",
 		"items": [{
 				"name": "save",
 				"value": "Draft",
 				"text": "Save Draft",
 				"css": "btn-white"
 			},
 			{
 				"name": "save",
 				"value": "submit",
 				"text": "Submit",
 				"css": "btn-primary"
 			}
 		]
 	}
 }
```

### Textbox Component
Textbox is the most common field component you will be using, good to take in short free text answer like a person name, which is equivalent to `<input type="text">` in HTML.
```
{
	"tag": "textbox",
	"prop": {
		"required": 1,
		"showinbackendlist": "1",
		"csv_label": "Participant Name",
		"hint": "",
		"value": "",
		"name": "fullname",
		"error": "Participant full name is required."
	}
}
```
### Number Component
Number is similar to `textbox` field component except it only take in number value (integer and floating point too), which is equivalent to `<input type="number">` in HTML. 
```
{
	"tag": "number",
	"prop": {
		"required": 1,
		"showinbackendlist": "1",
		"csv_label": "Total Customer",
		"hint": "This is a custom number only input",
		"value": "",
		"name": "totalCustomer",
		"error": ""
	}
}
```
### URL Component
```
{
	"tag": "url",
	"prop": {
		"required": 1,
		"csv_label": "Youtube",
		"hint": "This video should be less than 2 min on introduction about your project and each team members. Upload the video to youtube and set it public/unlisted, then paste the link here.",
		"value": "",
		"name": "urlYoutube",
		"error": ""
	}
}
```

### Email Component
```
{
	"tag": "email",
	"prop": {
		"required": 0,
		"showinbackendlist": "0",
		"csv_label": "Participant Email",
		"hint": "",
		"value": "",
		"name": "email",
		"error": "Participant email address is required."
	}
}
```

### Phone Component
```
{
	"tag": "phone",
	"prop": {
		"required": 1,
		"showinbackendlist": "0",
		"csv_label": "Participant Contact",
		"hint": "Please insert not less than 7 digit number only",
		"value": "",
		"name": "f1phone",
		"error": "Participant contact number is required."
	}
}
```

### Textarea Component
### Boolean Button Component
```
{
	"tag": "booleanButton",
	"prop": {
		"required": 0,
		"showinbackendlist": "0",
		"csv_label": "Yes Or No",
		"hint": "",
		"value": "",
		"name": "yesOrNo",
		"error": ""
	}
}
```

### List Component
```
{
	"tag": "list",
	"prop": {
		"required": 1,
		"showinbackendlist": "1",
		"csv_label": "Category",
		"hint": "This is a custom list",
		"value": "",
		"name": "category",
		"error": "Please specify the category you are interested in.",
		"text": "Select",
		"items": [{
				"text": "Social Track"
			},
			{
				"text": "Technology Track"
			}
		]
	}
}
```

### Checkbox Component
#### Inline
#### List
### Radio Button Component
### Google Place Component
### Upload Component
### Rating Component
> prop name must be prefix with `voted-`

```json
"1": {
    "tag": "group",
    "prop": {
        "css": ""
    },
    "members": [
        {
            "tag": "label",
            "prop": {
                "required": 1,
                "for": "voted-satisfaction",
                "value": "Rate the satisfaction of customer support"
            }
        },
        {
            "tag": "rating",
            "prop": {
                "required": 1,
                "showinbackendlist": "1",
                "csv_label": "Satisfaction Rating",
                "hint": "",
                "value": "",
                "name": "voted-satisfaction",
                "label_low": "Strongly Disagree",
                "label_high": "Strongly Agree",
                "error": ""
            }
        }
    ]
},
```
### Mapped Component

## Stage Pipeline
F7 form comes with simple process pipeline. It is linear and new application start with the first stage, e.g. `Application` in example below.

![](https://user-images.githubusercontent.com/5336690/82217092-84a21380-994c-11ea-87e8-fdd7e84011a0.png)
``` json
[
    {
        "key": "application",
        "title": "Application"
    },
    {
        "key": "screening",
        "title": "Screening"
    },
    {
        "key": "inprogram",
        "title": "In Program"
    },
    {
        "key": "rejected",
        "title": "Rejected"
    }
]
```
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

## Extra Settings
Extra setting can be set at `Extra` field in form.

### View Controls
#### Hide user previous submissions
`hideMySubmissions`

#### Hide other forms of the same intake
`hideAvailableFormForIntake`

#### Override OK button in view submission page
`publishViewOkButton`

### Sample:
```json
{
	"hooks": [{
		"code": "onNotifyAfterSubmitForm",
		"call": "HubSim::hookNotifyAfterSubmitForm"
	}, {
		"code": "onNotifyAfterChangedSubmit2Draft",
		"call": "HubSim::hookNotifyAfterChangedSubmit2Draft"
	}],
	"viewControls": {
		"hideMySubmissions": true,
		"hideAvailableFormForIntake": true,
		"publishViewOkButton": {
			"url": "\/sea\/frontend\/manage",
			"urlParams": [{
				"key": "id",
				"map": "startup_id"
			}]
		}
	}
}
```
## Others
### Preset value from URL
### Conditional form
### Override layout design

## Known Issues
This module was heavily hard coded by a developer and lots of refactor and rework needed to set it right. Here are few issues to take notes:
  - File upload is hardcoded to 10MB only and involved session
  - Survey is hardcoded to the database table
  - views generated are buggy
