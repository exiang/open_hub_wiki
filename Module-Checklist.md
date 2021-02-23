Feel free to use the following template as a guide to ensure the completeness of your module before publish it.

| No | Items  | Checked |
| -- |--|--|
| 1 | make sure `doOrganizationsMerge()` and `doIndividualsMerge()` is implemented to support organization and individual merge (if you have dataset in module that link to `organization` and `individual`) |  |
| 2 | make sure `getBackendAdvanceSearch()` is supported to provide better user experience for admin to search your dataset from header bar | |
| 3 | make sure all tables are created using `installDb()` function | |
| 4 | make sure updated `config/about.yaml` is updated with your details | |
| 5 | make sure all module public variables are exposed in `config/base.php` for convenience of admin to refer to | |
| 6 | make sure`getAsService()` is implemented to expose functionality to member in cpanel (if your module is a service) | |
| 7 | make sure `getDashboardViewTabs()` is implemented  to expose functionality to admin in backend dashboard page (if required) | |
| 8 | make sure module do not edit any of the other core table (e.g. `organization`, `individual`, `event`) as well as others module tables structure. Use meta to extends. | |
| 9 | make sure `Setting::setSetting()` is used to create and store configuration instead of using a custom table  | | 
| 10 | make sure all static texts in module are `i18n` ready using `Yii::t('MODULE_CODE', 'Your text string here')` | |