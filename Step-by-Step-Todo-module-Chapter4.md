## Chapter 4
In this chapter, we explore how to build web API interface for TODO module. We will be creating a public API where external developers can query a list of TODOs of a specific organization.

### Web API
WAPI (Web API) is a default module ship with OpenHub. WAPI helps you easily setup and test your module API functions thru Swagger interface.

![](https://user-images.githubusercontent.com/5336690/74059169-7fb2f700-4a22-11ea-830f-f6ce7649cbd8.png)

### Create a getTodos Web API
1. Firstly, create a yaml file to catalog our APIs at `protected/modules/todo/data/api/todo.yaml`:

```yaml
swagger: '2.0'
info:
  description: Provides information of todos.
  version: "v1"
  title: Todo
  # put the contact info for your development or API team
  contact:
    email: yee.siang@mymagic.my

  license:
    name: Apache 2.0
    url: http://www.apache.org/licenses/LICENSE-2.0.html

# tags are used for organizing operations
tags:
- name: public
  description: Public API calls available to any developers
- name: admin
  description: Secured admin only API calls
- name: internal
  description: Internal API calls available to in-house developers only
paths:
  /getTodos:
    post:
      tags:
      - public
      summary: Get list of TODOs of an organization
      description: Get a list of TODOs of an organization, up to a limit or records
      consumes:
      - application/x-www-form-urlencoded
      parameters:
      - in: formData
        name: organizationId
        type: integer
        description: Organization ID to filter with.
        required: false
        x-example: 999
      - in: formData
        name: limit
        type: integer
        description: Limit records to fetch
        default: 100
      produces:
      - application/json
      responses:
        200:
          description: OK
basePath: /v1
# Added by API Auto Mocking Plugin
host: api-central.mymagic.my
# Added by API Auto Mocking Plugin
schemes:
 - https
```

2. Create a file `protected/modules/todo/actions/wapi/V1Controller/getTodos.php` with content:
```php
<?php

class getTodos extends Action
{
    public function run()
    {
        $mode = 'public';

        $meta = array();
        $limit = Yii::app()->request->getPost('limit');
        $organizationId = Yii::app()->request->getPost('organizationId');

        $limit = empty($limit) ? 100 : $limit;

        $meta['input']['limit'] = $limit;
        $meta['input']['organizationId'] = $organizationId;

        try {
	    $organization = Organization::model()->findByPk($organizationId);
            $tmps = HubTodo::getTodos($organization, $limit);

            if (!empty($tmps['items'])) {
                foreach ($tmps['items'] as $tmp) {
                    $result[] = $tmp->toApi(array('config' => array()));
                }
            }

            if (Yii::app()->params['dev']) {
                $meta['output']['sql'] = $tmps['sql'];
            }

            $this->getController()->outputSuccess($result, $meta);
        } catch (Exception $e) {
            $this->getController()->outputFail($e->getMessage(), $meta);
        }
    }
}
```

> Remember we created `getTodos($organization, $limit)` in `HubTodo` model in Chapter 3? Centralize your code in this way might seems troublesome at first but allow easier reference to it as your module getting more complicated

3. Go to `https://hubd.mymagic.my/wapi/swagger?code=todo&format=yaml&module=todo`, your web API is now up and running, ready to test.
![](https://user-images.githubusercontent.com/5336690/74060699-7ecf9480-4a25-11ea-8358-021d2cda4291.png)

4. Click `Execute` after inserted all the parameters for result
![](https://user-images.githubusercontent.com/5336690/74061274-a8d58680-4a26-11ea-9d10-8b9ac93c6936.png)

> Copy `toApi()` function from `protected/modules/todo/models/TodoBase.php` to  `protected/modules/todo/models/Todo.php` to make changes to output data.

> For more information on Web API, please refer to [Module Web API](Module-Web-API)

## Next Chapter
[Chapter 5](Step-by-Step-Todo-module-Chapter5) - Advance Use Case