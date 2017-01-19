---
title: API | MadKudu

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='https://app.madkudu.com/signup?plan=zapier'>Sign up for an API Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the MadKudu API.

<!-- We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right. -->

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.madkudu.com/v1/ping" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

> Make sure to replace `QUJDRDEyMzQ6` with the correct value.

The MadKudu API uses HTTP Basic Auth and requires using HTTPS on all API calls.

Your API Key should be used as the basic auth username. You do not need to provide a password.

For example, if your user’s API key was `ABCD1234`, you need to Base64 encode the string `ABCD1234:` (Please note the colon at the end) and prepend the string `Basic ``. In this case, this would result in a final header of:

`Authorization: Basic QUJDRDEyMzQ6`

<aside class="notice">
The rest of this documentation assumes that you pass the authorization header with every call
</aside>

# Behavioral API

# Deprecated

## Predict

```shell
curl "https://api.madkudu.com/v1/predict" \
  -H "Authorization: Basic QUJDRDEyMzQ6" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{"email": "elon@tesla.com"}'
```

> The API response will look like this:

```json
{
  "predictions": {
    "customer_fit": {
      "segment": "very good"
    }
  }
}
```

> where segment can be "low", "medium", "good", "very good" or "not enough information"

Returns the prediction results for a given email / domain

### HTTP Request

`POST https://api.madkudu.com/v1/predict`

`POST https://api.madkudu.com/v1/predictions`

### Query Parameters

Parameter | Description
--------- | -----------
email | **string (required)**
domain | **string (required)**
company | object
person | object

You can use either one of `email` or `domain` (if you supply both, we use `email`).

Additionally, if you already get them, you can pass the Clearbit `company` or `person` directly (see example on the right)

> Example of body if you already have the company and person information:

```
{
    "email": "jane.killian@oracle.com",
    "domain": "oracle.com",
    "company": {},
    "person": {}
}
```

> where `company` and `person` are the JSON responses from the Clearbit API.


# Utilities

## Ping

```shell
curl "https://api.madkudu.com/v1/ping"
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

> If the authorization is valid, the API will return:

```json
{
  "status": "ok"
}
```

This endpoint lets you test your credentials

### HTTP Request

`GET https://api.madkudu.com/v1/ping`
