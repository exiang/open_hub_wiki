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

In Postman interface:

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
OpenHub output JSON in its own standard format like:
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
You may enable API AUTH in `protected/.env` file to apply username and password for every API call to WAPI.

`ENABLE_API_AUTH`
`API_USERNAME`
`API_PASSWORD`

## Module API
More information available at [Module Web API](Module-Web-API)