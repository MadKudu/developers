---
title: API | MadKudu

language_tabs:
  - shell: Shell
  - javascript: Node.js

toc_footers:
  - <a href='https://app.madkudu.com/signup?plan=zapier'>Sign up for an API Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the MadKudu API.

- All requests should be made using *https://*.
- All response bodies, including errors are encoded in JSON.

We have official language bindings in:

- [Node.js](https://github.com/MadKudu/madkudu-node)

You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.madkudu.com/v1/ping" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
# Make sure to replace `QUJDRDEyMzQ6` with the correct value.
```

```javascript
var madkudu = require('@madkudu/madkudu-node')('your_api_key');
```

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
curl "https://api.madkudu.com/v1/persons?email=paul@madkudu.com" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

```javascript
madkudu.person.find({ email: 'paul@madkudu.com' })
    .then(function (person) {
        console.log(person);
    });
});
```

```json
{
  "email": "paul@madkudu.com",
  "object_type": "person",
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
        { "feature": "Software industry", "value": true, "type": "positive"}
      ]
    }
  },
  "company": {
    "properties": {
      "name": "MadKudu Inc",
      "domain": "madkudu.com",
      "location": {
        "state": "California",
        "state_code": "CA",
        "country": "United States",
        "country_code": "US",
        "tags": ["english_speaking", "high_gdp_per_capita"]      
      },
      "number_of_employees": 17000,
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
email | **string (required)** | The email of the lead you would like to retrieve

### Properties

Attribute | Type | Description
--------- | ---- | -----------
properties.first_name | string | First name of the person
properties.last_name | string | Last name of the the person
properties.domain | string | The domain associated with this email address
properties.is_student | boolean | true if the email is identified as a student email
properties.is_spam | boolean | true if the email is identified as a spam email
properties.is_personal_email | boolean | true if the email is identified as a personal (or disposable) email address (e.g. gmail.com)
properties.customer_fit.segment | string | How close is this customer from your ideal customer profile?
properties.customer_fit.top_signals | array | An array of signals explaining the customer_fit segment

### Company properties

Attribute | Type | Description
--------- | ---- | -----------
company.name | string | The name of the company associated with this person
company.domain | string | The domain of the company associated with this person
company.location | object | The location of the company's headquarters.
company.location.state | string | The headquarters' state name
company.location.state_code | string | The headquarters' two-character state code
company.location.country | string | The headquarters's country
company.location.country_code | string | The headquarters's two-character country code
company.location.tags | array | An array of tags describing the location
company.number_of_employees | number | The number of employees at the company
company.industry | string | The industry of the company


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
