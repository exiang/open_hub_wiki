As with Coding standards naming consistency is very important in OpenHub, thus there are conventions that every OpenHub contributor should follow.

## Naming conventions

### Controllers & actions
OpenHub controllers follow these naming conventions:

Prefix controller with resource name in singular form (e.g. `OrganizationController`, `IndividualController`, `EventController`);

Action name should be clear and concise (e.g. `actionCreate`(), `actionBulkInsert`() are good examples, but `actionForm`() or `actionProcess`() is not and thus should be avoided).

We have some standard action names: - `actionList` : display the listing - `actionCreate` : show language creation form page and handle its submit - `actionUpdate` : show language edit form page and handle its submit - `actionDelete` : delete an item