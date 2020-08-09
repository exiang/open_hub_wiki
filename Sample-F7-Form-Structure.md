``` json
{
    "1": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "csv_label": "",
                    "for": "startup",
                    "hint": "",
                    "value": "Startup Team / Company / Project name:"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Startup Name",
                    "hint": "A working name for your team / project is also acceptable.",
                    "value": "",
                    "name": "startup",
                    "error": "Startup Name field is required",
                    "text": "choose your team / company / project",
                    "items": "",
                    "model_mapping": {
                        "startup": "Organization"
                    }
                }
            }
        ]
    },
    "3": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "myPersona",
                    "value": "Persona"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "1",
                    "csv_label": "Persona",
                    "hint": "",
                    "value": "",
                    "name": "myPersona",
                    "error": "How should we clasify you",
                    "text": "Select",
                    "items": "",
                    "model_mapping": {
                        "myPersona": "persona"
                    }
                }
            }
        ]
    },
    "4": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "theStartupStage",
                    "value": "Your Startup Stages"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "csv_label": "Startup Stage",
                    "hint": "At which stages your startup current is",
                    "value": "",
                    "name": "theStartupStage",
                    "text": "Select",
                    "error": "",
                    "model_mapping": {
                        "theStartupStage": "startupStage"
                    }
                }
            }
        ]
    },
    "5": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "primaryCluster",
                    "value": "Cluster"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "csv_label": "Cluster",
                    "hint": "Select one primary Cluster",
                    "name": "primaryCluster",
                    "text": "Select",
                    "error": "",
                    "model_mapping": {
                        "primaryCluster": "cluster"
                    }
                }
            }
        ]
    },
    "6": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "primaryIndustry",
                    "value": "Industry"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "csv_label": "Industry",
                    "hint": "Select one primary Industry",
                    "name": "primaryIndustry",
                    "text": "Select",
                    "error": "",
                    "model_mapping": {
                        "primaryIndustry": "industry"
                    }
                }
            }
        ]
    },
    "7": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "primarySdg",
                    "value": "SDG"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "csv_label": "SDG",
                    "hint": "Select one primary SDG",
                    "name": "primarySdg",
                    "text": "Select",
                    "error": "",
                    "model_mapping": {
                        "primarySdg": "sdg"
                    }
                }
            }
        ]
    },
    "10": {
        "tag": "divider",
        "prop": {
            "css": "",
            "style": "",
            "required": 0,
            "size": 2
        }
    },
    "14": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "star",
                    "value": "Rating"
                }
            },
            {
                "tag": "rating",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "1",
                    "csv_label": "Rating",
                    "hint": "Rate your experience",
                    "value": "",
                    "name": "star",
                    "error": ""
                }
            }
        ]
    },
    "15": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "location",
                    "value": "HQ Location"
                }
            },
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
        ]
    },
    "16": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "totalCustomer",
                    "value": "Total customer"
                }
            },
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
        ]
    },
    "17": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "category",
                    "value": "Which categories are you working or interested in?"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "1",
                    "csv_label": "Category",
                    "hint": "This is a custom list",
                    "value": "",
                    "name": "category",
                    "error": "Please specify the category you are insterested in.",
                    "text": "Select",
                    "items": [
                        {
                            "text": "Social Track"
                        },
                        {
                            "text": "Technology Track"
                        }
                    ]
                }
            }
        ]
    },
    "18": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "segment",
                    "value": "Who is your main customer segment"
                }
            },
            {
                "tag": "radio",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "1",
                    "csv_label": "Segment",
                    "hint": "This is a custom radio",
                    "value": "",
                    "name": "segment",
                    "error": "",
                    "text": "Select",
                    "items": [
                        {
                            "text": "Youth"
                        },
                        {
                            "text": "Family"
                        },
                        {
                            "text": "Senior"
                        }
                    ]
                }
            }
        ]
    },
    "19": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "regions",
                    "value": "Which regions your business focus at?"
                }
            },
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
                    "items": [
                        {
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
        ]
    },
    "20": {
        "tag": "divider",
        "prop": {
            "css": "",
            "style": "",
            "required": 0,
            "size": 2
        }
    },
    "22": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "uploadPitchdeck",
                    "value": "Upload Pitch Deck / Business Plan (PDF)"
                }
            },
            {
                "tag": "upload",
                "prop": {
                    "required": 1,
                    "csv_label": "",
                    "hint": "Only PDF format and the file size must be less than 10Mb.",
                    "value": "",
                    "name": "uploadPitchdeck",
                    "error": ""
                }
            }
        ]
    },
    "23": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "urlYoutube",
                    "value": "Youtube URL to application video"
                }
            },
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
        ]
    },
    "24": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "heardFrom",
                    "value": "Heard From"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "csv_label": "",
                    "hint": "How you know about us?",
                    "value": "",
                    "name": "heardFrom",
                    "error": "",
                    "model_mapping": {
                        "heardFrom": "heard"
                    }
                }
            }
        ]
    },
    "30": {
        "tag": "divider",
        "prop": {
            "css": "",
            "style": "",
            "required": 0,
            "size": 2
        }
    },
    "31": {
        "tag": "headline",
        "prop": {
            "css": "",
            "style": "",
            "required": 0,
            "size": 2,
            "text": "Participant Details"
        }
    },
    "33": {
        "tag": "section",
        "prop": {
            "name": "accordion-33",
            "css": "",
            "class": "",
            "size": 3,
            "text": "Participant 1",
            "mode": "accordion",
            "accordionOpen": "in"
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1name",
                    "value": "Full Name (as per IC / Passport):"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "1",
                    "csv_label": "Participant 1 Name",
                    "hint": "",
                    "value": "",
                    "name": "f1name",
                    "error": "Participant 1 full name is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1ic",
                    "value": "IC / Passport Number:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 1 IC / Passport No.",
                    "hint": "",
                    "value": "",
                    "name": "f1ic",
                    "error": "Participant 1 IC / Passport is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1nationality",
                    "value": "Nationality:"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 1 Nationality",
                    "hint": "",
                    "value": "",
                    "name": "f1nationality",
                    "model_mapping": {
                        "f1nationality": "country",
                        "hideOthers": true
                    },
                    "error": "Participant 1 Nationality is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1race",
                    "value": "Race:"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 1's Race",
                    "hint": "",
                    "value": "",
                    "name": "f1race",
                    "text": "Please select",
                    "items": [
                        {
                            "text": "Malay"
                        },
                        {
                            "text": "Chinese"
                        },
                        {
                            "text": "Indian"
                        },
                        {
                            "text": "Bumiputera Sarawak"
                        },
                        {
                            "text": "Bumiputera Sabah"
                        },
                        {
                            "text": "Other"
                        }
                    ],
                    "error": "Participant 1 Race is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1gender",
                    "value": "Gender:"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Gender",
                    "hint": "",
                    "value": "",
                    "name": "f1gender",
                    "error": "Gender is required.",
                    "text": "Select your gender",
                    "items": "",
                    "model_mapping": {
                        "f1gender": "Gender"
                    }
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1phone",
                    "value": "Contact Number:"
                }
            },
            {
                "tag": "phone",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 1 Contact",
                    "hint": "Please insert not less than 7 digit number only",
                    "value": "",
                    "name": "f1phone",
                    "error": "Participant 1 contact number is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1email",
                    "value": "Email Address:"
                }
            },
            {
                "tag": "email",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Participant Email",
                    "hint": "",
                    "value": "",
                    "name": "f1email",
                    "error": "Participant 1 email address is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1StudentId",
                    "value": "Student ID:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Student ID",
                    "hint": "",
                    "value": "",
                    "name": "f1StudentId",
                    "error": "Participant 1 Student ID is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1University",
                    "value": "University:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "University",
                    "hint": "",
                    "value": "",
                    "name": "f1University",
                    "error": "Participant 1 University is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f1Faculty",
                    "value": "Faculty:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "0",
                    "csv_label": "Faculty",
                    "hint": "",
                    "value": "",
                    "name": "f1Faculty",
                    "error": "Participant 1 Faculty is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "f1Allergy",
                    "value": "Food Allergy:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Food Allergy",
                    "hint": "",
                    "value": "",
                    "name": "f1Allergy",
                    "error": "Participant 1 has any food allergy?"
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "f1Accommodation",
                    "value": "Required Accommodation:"
                }
            },
            {
                "tag": "booleanButton",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Accomodation",
                    "hint": "",
                    "value": "",
                    "name": "f1Accommodation",
                    "error": "Participant 1 needs accomodation?"
                }
            },
            {
                "tag": "break",
                "prop": {}
            }
        ]
    },
    "34": {
        "tag": "section",
        "prop": {
            "name": "accordion-34",
            "css": "",
            "class": "",
            "size": 3,
            "text": "Participant 2",
            "mode": "accordion",
            "accordionOpen": "out"
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2name",
                    "value": "Full Name (as per IC / Passport):"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "1",
                    "csv_label": "Participant 2 Name",
                    "hint": "",
                    "value": "",
                    "name": "f2name",
                    "error": "Participant 2 full name is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2ic",
                    "value": "IC / Passport Number:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 2 IC / Passport No.",
                    "hint": "",
                    "value": "",
                    "name": "f2ic",
                    "error": "Participant 2 IC / Passport is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2nationality",
                    "value": "Nationality:"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 2 Nationality",
                    "hint": "",
                    "value": "",
                    "name": "f2nationality",
                    "model_mapping": {
                        "f2nationality": "country",
                        "hideOthers": true
                    },
                    "error": "Participant 2 Nationality is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2race",
                    "value": "Race:"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 2's Race",
                    "hint": "",
                    "value": "",
                    "name": "f2race",
                    "text": "Please select",
                    "items": [
                        {
                            "text": "Malay"
                        },
                        {
                            "text": "Chinese"
                        },
                        {
                            "text": "Indian"
                        },
                        {
                            "text": "Bumiputera Sarawak"
                        },
                        {
                            "text": "Bumiputera Sabah"
                        },
                        {
                            "text": "Other"
                        }
                    ],
                    "error": "Participant 2 Race is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2gender",
                    "value": "Gender:"
                }
            },
            {
                "tag": "list",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Gender",
                    "hint": "",
                    "value": "",
                    "name": "f2gender",
                    "error": "Gender is required.",
                    "text": "Select your gender",
                    "items": "",
                    "model_mapping": {
                        "f2gender": "Gender"
                    }
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2phone",
                    "value": "Contact Number:"
                }
            },
            {
                "tag": "phone",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 2 Contact",
                    "hint": "Please insert not less than 7 digit number only",
                    "value": "",
                    "name": "f2phone",
                    "error": "Participant 12contact number is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2email",
                    "value": "Email Address:"
                }
            },
            {
                "tag": "email",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Participant 2 Email",
                    "hint": "",
                    "value": "",
                    "name": "f2email",
                    "error": "Participant 2 email address is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2StudentId",
                    "value": "Student ID:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Student ID",
                    "hint": "",
                    "value": "",
                    "name": "f2StudentId",
                    "error": "Participant 2 Student ID is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2University",
                    "value": "University:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "University",
                    "hint": "",
                    "value": "",
                    "name": "f2University",
                    "error": "Participant 2 University is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "f2Faculty",
                    "value": "Faculty:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Faculty",
                    "hint": "",
                    "value": "",
                    "name": "f2Faculty",
                    "error": "Participant 2 Faculty is required."
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "f2Allergy",
                    "value": "Food Allergy:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Food Allergy",
                    "hint": "",
                    "value": "",
                    "name": "f2Allergy",
                    "error": "Participant 2 has any food allergy?"
                }
            },
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "f2Accommodation",
                    "value": "Required Accommodation:"
                }
            },
            {
                "tag": "booleanButton",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Accomodation",
                    "hint": "",
                    "value": "",
                    "name": "f2Accommodation",
                    "error": "Participant 2 needs accomodation?"
                }
            },
            {
                "tag": "break",
                "prop": {}
            }
        ]
    },
    "70": {
        "tag": "divider",
        "prop": {
            "css": "",
            "style": "",
            "required": 0,
            "size": 2
        }
    },
    "71": {
        "tag": "group",
        "prop": {
            "css": "inline"
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 1,
                    "for": "condition1",
                    "value": "Select an option"
                }
            },
            {
                "tag": "radio",
                "prop": {
                    "required": 1,
                    "showinbackendlist": "1",
                    "csv_label": "Your Hobby",
                    "hint": "This is a condition",
                    "value": "",
                    "css": "inline",
                    "name": "condition1",
                    "error": "",
                    "text": "Select",
                    "items": [
                        {
                            "text": "Food Lover"
                        },
                        {
                            "text": "Movie Lover"
                        }
                    ]
                }
            }
        ]
    },
    "72": {
        "tag": "group",
        "prop": {
            "name": "choice-A",
            "css": "",
            "size": 3,
            "text": "Food Lover"
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "favRestaurant",
                    "value": "Favorite Restaurant:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Favorite Restaurant",
                    "hint": "",
                    "value": "",
                    "name": "favRestaurant",
                    "error": ""
                }
            },
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "favFruit",
                    "value": "Favorite Fruit:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Favorite Fruit",
                    "hint": "",
                    "value": "",
                    "name": "favFruit",
                    "error": ""
                }
            }
        ]
    },
    "73": {
        "tag": "group",
        "prop": {
            "name": "choice-B",
            "css": "",
            "size": 3,
            "text": "Movie Lover"
        },
        "members": [
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "favCinema",
                    "value": "Favorite Cinema:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Favorite Cinema",
                    "hint": "",
                    "value": "",
                    "name": "favCinema",
                    "error": ""
                }
            },
            {
                "tag": "label",
                "prop": {
                    "required": 0,
                    "for": "favMovie",
                    "value": "Favorite Movie:"
                }
            },
            {
                "tag": "textbox",
                "prop": {
                    "required": 0,
                    "showinbackendlist": "0",
                    "csv_label": "Favorite Movie",
                    "hint": "",
                    "value": "",
                    "name": "favMovie",
                    "error": ""
                }
            }
        ]
    },
    "90": {
        "tag": "group",
        "prop": {
            "css": ""
        },
        "members": [
            {
                "tag": "break",
                "prop": {}
            },
            {
                "tag": "break",
                "prop": {}
            }
        ]
    },
    "99": {
        "tag": "button",
        "prop": {
            "css1": "",
            "css2": "",
            "name": "Submit",
            "items": [
                {
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
    },
    "form_type": "vertical",
    "jscripts": [
        {
            "caller": "condition1",
            "action": "show",
            "items": [
                "favFruit",
                "favRestaurant"
            ],
            "condition": {
                "check": "Food Lover"
            }
        },
        {
            "caller": "condition1",
            "action": "show",
            "items": [
                "favMovie",
                "favCinema"
            ],
            "condition": {
                "check": "Movie Lover"
            }
        }
    ]
}
```