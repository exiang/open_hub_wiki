API is an important component in OpenHub to allow its services to be consumed and interact with by external party, namely:

  * **Developer** - external developer who applied for access thru online, like hackathon participants, app developers, startups.
  * **Partner** - fundamentally are external developer too but with access to additional scope as NDA and contract signed and enforced.
  * **User** - user can only access the API thru the above two entity: either partner or developer. Eg: a normal user that like to access his own information thru API has to register as a developer with us

## WAPI & Swagger
OpenHub default module WAPI stands for 'Web API', is enabled by default after installation and come with a web interface for developers to test their REST like API using [Swagger](https://swagger.io/). The interface is accessible thru `https://mydomain.com/wapi/swagger`.

![](https://user-images.githubusercontent.com/5336690/81764067-3f708280-9503-11ea-97cd-c095c00130aa.png)

### Swagger Definition in YAML
API definition is written according to swagger format and store as YAML file. Default API definition can be located at the following location:

* `protected/data/api/`
* `protected/modules/journey/api/journey.yaml`

``` json
swagger: '2.0'
info:
  description: Group of test APIs
  version: "v1"
  title: Test API
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
  /now:
    get:
      tags:
      - public
      description: |
        Return current GMT timestamp
      produces:
      - application/json
      responses:
        200:
          description: OK
  /sum:
    get:
      tags:
      - public
      description: |
        Test Sending GET Data with 2 input parameters, response will sum up these 2 input into number
      consumes:
        - application/x-www-form-urlencoded
      parameters: 
        - in: query
          name: num1
          type: number
          description: Number 1
          required: true
          default: 30
        - in: query
          name: num2
          type: number
          description: Number 2
          required: true
          default: 3
      produces:
      - application/json
      responses:
        200:
          description: OK
  /testPost:
    post:
      tags:
      - public
      description: |
        Test Sending POST Data with 2 input parameters, response will concat these input into a string
      consumes:
        - application/x-www-form-urlencoded
      parameters: 
        - in: formData
          name: var1
          type: string
          description: Variable 1
          required: true
          default: Hello
        - in: formData
          name: var2
          type: string
          description: Variable 2
          required: true
          default: World
      produces:
      - application/json
      responses:
        200:
          description: OK
  /secureTestPost:
    post:
      tags:
      - public
      description: |
        Test Sending POST Data with 2 input parameters, response will concat these input into a string, required api key
      consumes:
        - application/x-www-form-urlencoded
      parameters: 
        - in: formData
          name: var1
          type: string
          description: Variable 1
          required: true
          default: Hello
        - in: formData
          name: var2
          type: string
          description: Variable 2
          required: true
          default: World
      produces:
      - application/json
      security:
        - Bearer: []
      responses:
        200:
          description: OK
basePath: /v1
# Added by API Auto Mocking Plugin
host: api-central.mymagic.my
# Added by API Auto Mocking Plugin
schemes:
 - https
securityDefinitions:
  Bearer:
    type: apiKey
    name: Authorization
    in: header
```

## Postman
[Postman](https://www.postman.com/) is a handy FREE tool for you to test your API apart from the swagger interface above. 

Lest try it out using `test` API in OpenHub. In swagger interface:

![](https://user-images.githubusercontent.com/5336690/81627939-dd941800-9431-11ea-8240-0cb613acb1a7.png)

Translated into Postman interface:

![](https://user-images.githubusercontent.com/5336690/81627698-58a8fe80-9431-11ea-8922-b82dabc0a54c.png)

1. Add a New Request
2. Insert `https://api-hubd.mymagic.my/v1/testPost` and select `POST` to Request URL
3. As this API deal with POST data, click `Body` tab
    - Select `x-www-form-urlencoded`
    - Insert 'var1' to `KEY`, 'Hello' to `VALUE`
    - Insert 'var2' to `KEY`, 'World' to `VALUE`
4. Click `Send` button and receive the following returned output in SoJF (Standard OpenHub JSON Format):

``` json
{
    "status": "success",
    "meta": {
        "input": {
            "var1": "Hello",
            "var2": "World"
        }
    },
    "msg": "",
    "data": "Hello World"
}
```

## SoJF
SoJF stands for '**Standard OpenHub JSON Format**'. OpenHub output JSON in its own standard format. Please make sure you following this whenever you are output a JSON in not just WAPI, but in any module action too. 
 
``` json
{
    "status": "fail",
    "msg": "Unknown error, something go wrong"
}
```

or

``` json
{
    "status": "success",
    "meta": {
        "input": {
            "var1": "Hello",
            "var2": "World"
        }
    },
    "msg": "",
    "data": "Hello World"
}
```

or

``` json
{
  "status": "success",
  "meta": {
    "input": {
      "var1FromPost": "abc",
      "var2FromPost": "def",
      "output": {
        "total": 2
      }
    }
  },
  "msg": "Everything works fine",
  "data": [
    {
      "orderId": "99",
      "productTitle": "4K LED TV",
      "deliveryLocation": "Cyberjaya, Malaysia",
      "buyerName": "Allen Tan"
    },
    {
      "orderId": "88",
      "productTitle": "Playstation 4",
      "deliveryLocation": "KLIA, Malaysia",
      "buyerName": "Foo Bar"
    }
  ]
}
```

## API Gateway
MaGIC Central used [tyk.io](https://tyk.io), an open source API Gateway to expose API available for external developers. MaGIC do not have the capacity to host and maintain an API gateway internally. 

### TYK Backend
The backend can be access from https://admin.cloud.tyk.io/
### Catalog & Policy
* One Policy for one Catalog only. Make sure the policy name and catalog are corresponding to each others
* Do not remove/delete Policy!
* One API can attach to multiple policy, only API with authentication will show in the list

### Adding a new API
1. Add new API
    * apiName: `getAllLegalForms`
    * apiSlug: `getAllLegalForms`
    * targetUrl: `https://api-central.mymagic.my/v1/getAllLegalForms` (make sure it start with https, mapping https to http will not pass the POST value when called)
    * Authentication: `AUTH Token`
2. Add API to Policy
3. Test with Postman
4. Insert API into swagger.io
5. Export to unresolved json
6. Copy paste into the tyk.io related documentation under the catalog (delete if pre-exists)


## Security
### Protect WAPI Swagger
`TODO`
This interface should be accessible by your internal developers but not public. 


### Whitelist API Gateway IPs
`TODO`

### Use API AUTH
Enable this to protect all your API call in WAPI with [HTTP Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication). 

> Basic auth features no encryption or obfuscation beyond a base64 transport encoding. Usage of SSL is advised in order to ensure the confidentiality of login credentials.

Modify `protected/.env` file:
* `ENFORCE_API_SSL` to `true`
* `ENABLE_API_AUTH` to `true`
* `API_USERNAME` to `api` or any other name
* `API_PASSWORD` to a random secure password

When query in Postman, make sure you have selected `Basic Auth` and inserted the correct credential in `Authorization` tab. `HTTP/1.1 401 Unauthorized` is return when Basic Auth failed.

![](https://user-images.githubusercontent.com/5336690/81630930-66fb1880-9439-11ea-854e-ee75589aa931.png)

#### Troubleshoot
If test in browser and you keep getting popup asking for username and password, make sure the following code exists in your  `public_html/.htaccess`
```RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization},L]```



## Module API
API is supported in OpenHub module architecture. More information available at [Module Web API](Module-Web-API).
Module's API definition is stored in: `protected/modules/[moduleCode]/api/[moduleCode].yaml` and will automatically parsed by WAPI to display in the swagger interface.