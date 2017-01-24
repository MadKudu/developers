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

## Persons

```shell
curl "https://api.madkudu.com/v1/persons" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

```json
{
  "email": "paul@madkudu.com",
  "properties": {
    "first_name": "Paul",
    "last_name": "Cothenet",
    "domain": "madkudu.com",
    "is_student": false,
    "is_spam": false,
    "is_personal_email": false,
    "customer_fit": {
      "segment": "good",
      "top_signals": [
        { "feature": "Employees count", "value": "200", "type": "positive"},
        { "feature": "Software industry", "value": true, "type": "positive"},
      ]
    }
  },
  "company": {
    "name": "MadKudu Inc",
    "domain": "madkudu.com",
    "properties": {
      "location": {
        "state": "California",
        "state_code": "CA",
        "country": "United States",
        "country_code": "US",
        "tags": ["english_speaking", "high_gdp"]      
      },
      "number_of_employees": 7,
      "industry": "Software"             
    }
  }
}
```

### HTTP Request

`GET https://api.madkudu.com/v1/persons?email=:email`

### Query Parameters

Parameter | Description
--------- | -----------
first_name | **string (required)** | The email of the lead you would like to retrieve

### Properties

Attribute | Type | Description
--------- | ---- | -----------
first_name | string |
last_name | string |
domain | string | The domain associated with this email address
is_student | boolean | true if the email is identified as a student email
is_spam | boolean | true if the email is identified as a spam email
is_personal_email | boolean | true if the email is identified as a personal (or disposable) email address (e.g. gmail.com)
customer_fit | object |
customer_fit.segment | string |
customer_fit.top_signals | array |

### Company properties

Attribute | Type | Description
--------- | ---- | -----------
name | string | the
domain | string | The domain of the company associated with this person
location | object |
location.state | string |
location.state_code | string |
location.country | string |
location.country_code | string |
location.tags | array |
number_of_employees | number |
industry | string |


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
