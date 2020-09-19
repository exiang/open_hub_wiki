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
Section has 2 modes: default or accordion.

#### Default Section
```
{
    "tag": "section",
    "prop": {
        "name": "section-1",
        "css": "",
        "class": "",
        "text": "Normal Section"
    },

    "members":[
        {
            "tag": "html",
            "prop":{
                "value": "This is a section by default"
            }
            
        }
    ]
}
```
#### Accordion Section
Use `"accordionOpen": "in"` to open this accordion section by default. 
```
{
    "tag": "section",
    "prop": {
        "name": "accordion-1",
        "css": "",
        "class": "",
        "text": "Accordion Section 1",
        "mode": "accordion",
        "accordionOpen": "in"
    },
    "members":[
        {
            "tag": "html",
            "prop":{
                "value": "This is a section with accordion mode and expand by default"
            }
            
        }
    ]
},
```

### Group Component
### Label Component
### Headline Component
### Break Component
### Divider Component
### Button Component
<img width="387" alt="Screenshot 2020-08-09 at 9 18 21 PM" src="https://user-images.githubusercontent.com/5336690/89733121-deb30080-da85-11ea-96ab-9c934677fce5.png">

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
Number is similar to `textbox` field component except it only take in number value (integer and floating point too), which is equivalent to `<input type="number" step="any">` in HTML. 
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
<img width="767" alt="Screenshot 2020-08-09 at 9 15 56 PM" src="https://user-images.githubusercontent.com/5336690/89733082-998ece80-da85-11ea-82cd-712e6b0850cc.png">

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
```
{
	"tag": "textarea",
	"prop": {
		"required": 1,
		"showinbackendlist": "1",
		"csv_label": "Short Description",
		"hint": "",
		"value": "",
		"name": "shortDescription",
		"error": "Short Description is required."
	}
}
```
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
<img width="796" alt="Screenshot 2020-08-09 at 9 17 38 PM" src="https://user-images.githubusercontent.com/5336690/89733104-c7741300-da85-11ea-8f5c-5ffabea0552e.png">

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
<img width="683" alt="Screenshot 2020-08-09 at 9 12 02 PM" src="https://user-images.githubusercontent.com/5336690/89732997-10779780-da85-11ea-97c1-92b7f59ad889.png">

```
{
	"tag": "checkbox",
	"prop": {
		"required": 1,
		"isGroup": 1,
		"isInlineItems": 1,
		"showinbackendlist": "0",
		"csv_label": "Regions",
		"hint": "",
		"name": "regions",
		"error": "",
		"items": [{
				"text": "North Peninsula"
			},
			{
				"text": "Central Peninsula"
			},
			{
				"text": "South Peninsula"
			},
			{
				"text": "Eastcoast Peninsula"
			},
			{
				"text": "Sarawak"
			},
			{
				"text": "Sabah"
			}
		]
	}
}
```
#### List
<img width="378" alt="Screenshot 2020-08-09 at 9 13 50 PM" src="https://user-images.githubusercontent.com/5336690/89733024-3dc44580-da85-11ea-89a5-b043d3972369.png">

```
{
	"tag": "checkbox",
	"prop": {
		"required": 1,
		"isGroup": 1,
		"isInlineItems": 0,
		"showinbackendlist": "0",
		"csv_label": "Regions",
		"hint": "",
		"name": "regions",
		"error": "",
		"items": [{
				"text": "North Peninsula"
			},
			{
				"text": "Central Peninsula"
			},
			{
				"text": "South Peninsula"
			},
			{
				"text": "Eastcoast Peninsula"
			},
			{
				"text": "Sarawak"
			},
			{
				"text": "Sabah"
			}
		]
	}
}
```
### Radio Button Component
<img width="409" alt="Screenshot 2020-08-09 at 9 15 07 PM" src="https://user-images.githubusercontent.com/5336690/89733053-6b10f380-da85-11ea-8bc5-6520224c396b.png">

```
{
	"tag": "radio",
	"prop": {
		"required": 1,
		"showinbackendlist": "1",
		"csv_label": "Your Hobby",
		"hint": "Please elect either one",
		"value": "",
		"css": "inline",
		"name": "radio1",
		"error": "",
		"text": "Select",
		"items": [{
				"text": "Food Lover"
			},
			{
				"text": "Movie Lover"
			}
		]
	}
}
```

### Google Place Component
> This component has bug as it is hardcoded to appear only once per form. It's now at drawback mode displaying just text input box.

```php
 {
 	"tag": "googleplace",
 	"prop": {
 		"required": 1,
 		"showinbackendlist": "1",
 		"csv_label": "HQ Location",
 		"hint": "This is a google map input",
 		"value": "",
 		"name": "location",
 		"error": ""
 	}
 }
```
### Upload Component
<img width="795" alt="Screenshot 2020-08-09 at 9 15 52 PM" src="https://user-images.githubusercontent.com/5336690/89733088-a4e1fa00-da85-11ea-8be0-5275ee787c5e.png">

```
 {
 	"tag": "upload",
 	"prop": {
 		"required": 1,
 		"csv_label": "Pitch Deck",
 		"hint": "Only PDF format and the file size must be less than 10Mb.",
 		"value": "",
 		"name": "uploadPitchdeck",
 		"error": ""
 	}
 }
```
### Rating Component
<img width="577" alt="Screenshot 2020-08-09 at 9 16 02 PM" src="https://user-images.githubusercontent.com/5336690/89733071-8d0a7600-da85-11ea-8710-08545fc43f83.png">

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

### Tabular Component
Tabular is a group of supported inputs display in table form.
* It must has a predefined headers(columns) and rows.
* It only support the following components:
  * Label
  * Url
  * Email
  * Phone
  * Textbox
  * Number
  * Textarea
  * List
  * BooleanButton

<img width="1106" alt="Screenshot 2020-09-03 at 2 18 44 PM" src="https://user-images.githubusercontent.com/5336690/92078190-717a5d00-edf0-11ea-8393-2ecde9da6819.png">


```json
{
	"1": {
		"tag": "group",
		"prop": {
			"css": ""
		},
		"members": [{
			"tag": "label",
			"prop": {
				"required": 1,
				"csv_label": "",
				"for": "shareholders",
				"hint": "",
				"value": "List all equity ownership by shareholders name"
			}
		}, {
			"tag": "tabular",
			"prop": {
				"showinbackendlist": "0",
				"csv_label": "Shareholders",
				"hint": "Insert all your shareholders here. Must insert minimum 1.",
				"value": "",
				"name": "shareholders",
				"error": "Shareholders detail is required",
				"headers": [{
					"text": "#",
					"css": "text-center"
				}, {
					"required": 1,
					"text": "Name",
					"css": "text-center"
				}, {
					"required": 1,
					"text": "Percentage of Share",
					"hint": "in percentage %",
					"css": "text-center"
				}, {
					"required": 1,
					"text": "Amount invested\/Capital",
					"hint": "in USD",
					"css": "text-center"
				}, {
					"text": "Note",
					"hint": "Optional",
					"css": "text-center"
				}],
				"members": [{
					"tag": "row",
					"members": [{
						"tag": "label",
						"prop": {
							"required": 1,
							"value": "1",
							"css": "text-center"
						}
					}, {
						"tag": "textbox",
						"prop": {
							"required": 1,
							"csv_label": "Shareholder Name #1",
							"hint": "",
							"value": "",
							"name": "shareholders1-name",
							"error": ""
						}
					}, {
						"tag": "number",
						"prop": {
							"required": 1,
							"csv_label": "Share Percentage #1",
							"hint": "",
							"value": "",
							"name": "shareholders1-sharePercentage",
							"error": ""
						}
					}, {
						"tag": "number",
						"prop": {
							"required": 1,
							"csv_label": "Amount Invested #1",
							"hint": "",
							"value": "",
							"name": "shareholders1-amountInvested",
							"error": ""
						}
					}, {
						"tag": "textarea",
						"prop": {
							"required": 0,
							"csv_label": "Note #1",
							"hint": "",
							"value": "",
							"name": "shareholders1-note",
							"rows": 2,
							"error": ""
						}
					}]
				}, {
					"tag": "row",
					"members": [{
						"tag": "label",
						"prop": {
							"value": "2",
							"css": "text-center"
						}
					}, {
						"tag": "textbox",
						"prop": {
							"csv_label": "Shareholder Name #2",
							"hint": "",
							"value": "",
							"name": "shareholders2-name",
							"error": ""
						}
					}, {
						"tag": "number",
						"prop": {
							"csv_label": "Share Percentage #2",
							"hint": "",
							"value": "",
							"name": "shareholders2-sharePercentage",
							"error": ""
						}
					}, {
						"tag": "number",
						"prop": {
							"csv_label": "Amount Invested #2",
							"hint": "",
							"value": "",
							"name": "shareholders2-amountInvested",
							"error": ""
						}
					}, {
						"tag": "textarea",
						"prop": {
							"required": 0,
							"csv_label": "Note #2",
							"hint": "",
							"value": "",
							"name": "shareholders2-note",
							"rows": 2,
							"error": ""
						}
					}]
				}]
			}
		}]
	},
	"99": {
		"tag": "button",
		"prop": {
			"name": "Submit",
			"items": [{
				"name": "save",
				"value": "Draft",
				"text": "Save Draft",
				"css": "btn-white"
			}, {
				"name": "save",
				"value": "submit",
				"text": "Submit",
				"css": "btn-primary"
			}]
		}
	}
}
```


### Mapped Component

## Stage Pipeline
F7 form comes with simple process pipeline. It is linear and new application start with the first stage, e.g. `Application` in example below.

> All form must have a defined stages pipeline to function properly

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
