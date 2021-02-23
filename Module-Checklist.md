Feel free to use the following template as a guide to ensure the completeness of your module before publish it.

| No | Items  | Checked |
| -- |--|--|
| 1 | make sure `doOrganizationsMerge()` and `doIndividualsMerge()` is implemented to support organization and individual merge (if you have dataset in module that link to `organization` and `individual`) |  |
| 2 | make sure `getBackendAdvanceSearch()` is supported to provide better user experience for admin to search your dataset from header bar | |
| 3 | make sure all your tables are created using `installDb()` function | |
| 4 | make sure you updated `config/about.yaml` with your details | |
| 5 | make sure all module public variables are exposed in `config/base.php` for convenience of admin to refer to | |
| 6 | make sure you implemented `getAsService()` to expose functionality to member in cpanel (if your module is a service) | |
| 7 | make sure you implemented `getDashboardViewTabs()` to expose functionality to admin in backend dashboard page (if required) | |
