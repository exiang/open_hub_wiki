API is an important component in OpenHub to allow its services to be consumed and interact with by external party, namely:

  * **Developer** - external developer who applied for access thru online, like hackathon participants, app developers, startups.
  * **Partner** - fundamentally are external developer too but with access to additional scope as NDA and contract signed and enforced.
  * **User** - user can only access the API thru the above two entity: either partner or developer. Eg: a normal user that like to access his own information thru API has to register as a developer with us

## Swagger
OpenHub default module `WAPI` provide a web interface for developers to test their REST like API with the use of Swagger. It is accessible from `https://mydomain.com/wapi/swagger`.

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

```
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

## SoJF (Standard OpenHub JSON Format)
OpenHub output JSON in its own standard format. Please make sure you following this whenever you are output a JSON in not just WAPI, but in any module action too. 
 
```
{
    "status": "fail",
    "msg": "Unknown error, something go wrong"
}
```

or

```
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

```
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

## Security
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
API is supported in OpenHub module architecture. More information available at [Module Web API](Module-Web-API)