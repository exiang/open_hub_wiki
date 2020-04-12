OpenHub was conceived so that third-party modules could easily build upon its foundations, making it an extremely customizable startup ecosystem management software.

![image](https://user-images.githubusercontent.com/5336690/72774597-225b2f80-3c46-11ea-9548-2dbe5b97cc7f.png)

## Concepts
You should be familiar with PHP and Object-Oriented Programming before attempting to write your own module.

A module is an extension to OpenHub that enables any developer to add the following:

  * Provide additional functionality to OpenHub.
  * Communicate with other services (Acuity Scheduling, Futurelab, Envoy).
  * Expose additional functionality thru API interface.

## MVC as root
This is the same principle as the Model>View>Controller (MVC) architecture, only in a simpler and more accessible way.

### Model
A model represents the application’s behavior: data processing, database interaction, etc.

It describes or contains the data that have been processed by the application. It manages this data and guarantees its integrity.

### View
A view is the interface with which the user interacts.

Its first role is to display the data that has been provided by the model. Its second role is to handle all the actions from the user (mouse click, element selection, buttons, etc.), and send these events to the controller.

The view does not do any processing; it only displays the result of the processing performed by the model, and interacts with the user.

###  Controller
The Controller manages synchronization events between the Model and the View, and updates both as needed. It receives all the user events and triggers the actions to perform.

If an action needs data to be changed, the Controller will “ask” the Model to change the data, and in turn the Model will notify the View that the data has been changed, so that the View can update itself.

## Yii Framework
Using a proven and popular open-source framework will allow us to focus on our core business code (managing startup, tracking founder activities, etc.) with greater efficiency, while enjoying the stability of a globally recognized framework.