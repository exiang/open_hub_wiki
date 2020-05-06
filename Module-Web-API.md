WAPI (Web API) is a default module shipped with OpenHub. WAPI helps you easily setup and test your module API functions thru Swagger interface.
![](https://user-images.githubusercontent.com/5336690/74059169-7fb2f700-4a22-11ea-830f-f6ce7649cbd8.png)

Assume our module here is `demo`
Wapi support modularization architecture and able to read your module's swagger definition. `protected/modules/wapi/V1Controller.php` has been modified to auto load `protected/modules/demo/actions/wapi/V1Controller/*.php` for api action

### Step 1: Define API YAML
Create a yaml file to catalog your module APIs at `protected/modules/demo/data/api/demo.yaml`.

An example of YAML looks like the following:
```yaml
swagger: '2.0'
info:
  description: Demo module related API.
  version: "v1"
  title: Demo
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
  /sayHello:
    post:
      tags:
      - public
      summary: Output friendly hello message
      description: An example to output hello message with the input name
      consumes:
      - application/x-www-form-urlencoded
      parameters:
      - in: formData
        name: guestName
        type: integer
        description: Name of the guest we like to say hello to
        required: true
        x-example: World
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

### Step 2: Create WAPI Actions
Your API action sit at `protected/modules/demo/actions/wapi/V1Controller/sayDemoHello.php`
```php

class sayDemmoHello extends Action
{
	public function run()
	{
		$meta = array();
		$guestName = Yii::app()->request->getPost('guestName');
                $meta['input']['guestName'] = $guestName;
		
                try {
			$data = sprintf('Hello %s', $guestName)

			$this->getController()->outputSuccess($data, $meta);
		} catch (Exception $e) {
			$this->getController()->outputFail($e->getMessage(), $meta);
		}
	}
}
```

### Outputting JSON
`V1Controller` came with a few helpful functions for you to output JSON in OpenHub data format.

```php
public function outputMessage($msg, $meta = array())
{
    $this->outputJson('', $msg, 'success', $meta);
}
```

```php
public function outputSuccess($data, $meta = array(), $msg = '')
{
    $this->outputJson($data, $msg, 'success', $meta);
}
```

```php
public function outputFail($msg, $meta = array())
{
    $this->outputJson($data, $msg, 'fail', $meta);
}
```

```php
public function outputPipe($result)
{
    $this->outputJson($result['data'], $result['msg'], ($result['status'] == 'success' || $result['success'] == true) ? true : false, $result['meta']);
}
```
```


