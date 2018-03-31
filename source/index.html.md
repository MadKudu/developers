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
const madkudu = require('@madkudu/madkudu-node')('your_api_key');
```

The MadKudu API uses HTTP Basic Auth and requires using HTTPS on all API calls.

Your API Key should be used as the basic auth username. You do not need to provide a password.

For example, if your user’s API key was `ABCD1234`, you need to Base64 encode the string `ABCD1234:` (Please note the colon at the end) and prepend the string `Basic ``. In this case, this would result in a final header of:

`Authorization: Basic QUJDRDEyMzQ6`

<aside class="notice">
The rest of this documentation assumes that you pass the authorization header with every call
</aside>

# Demographics API

## Companies

Our Company API lets you lookup a company profile via a domain name. It returns the predictive customer fit of the company (aka. demographics score), the top signals behind this score (ie. why is this company a good fit or not), and some light demographics information (eg. number of employees, industry, etc.).

```shell
curl "https://api.madkudu.com/v1/companies?domain=madkudu.com" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

```javascript
madkudu.company.find({ domain: 'madkudu.com' })
    .then(function (company) {
        console.log(company);
    });
});
```

```json
{
    "object_type": "company",
    "domain": "madkudu.com",
    "properties": {
        "name": "MadKudu",
        "domain": "madkudu.com",
        "location": {
            "state": "California",
            "state_code": "CA",
            "country": "United States",
            "country_code": "US",
            "tags": [
                "english_speaking",
                "high_gdp_per_capita"
            ]
        },
        "number_of_employees": 10,
        "industry": "Internet Software & Services",
        "customer_fit": {
            "segment": "good",
            "score": 37,
            "top_signals": [
                {
                    "name": "employee count",
                    "value": 10,
                    "type": "negative"
                },
                {
                    "name": "based in a country where gdp per capita",
                    "value": "high",
                    "type": "positive"
                },
                {
                    "name": "capital raised",
                    "value": 1370000,
                    "type": "positive"
                },
                {
                    "name": "belongs to industry where revenue per employee",
                    "value": "high",
                    "type": "positive"
                },
                {
                    "name": "number of tech found on website",
                    "value": 10,
                    "type": "positive"
                },
                {
                    "name": "web traffic volume",
                    "value": "low",
                    "type": "negative"
                }
            ]
        }
    }
}
```

### HTTP Request

`GET https://api.madkudu.com/v1/companies?domain=:domain`

### Query Parameters

Parameter | Description
--------- | -----------
domain | **string (required)** | The domain name of the company you would like to retrieve

### Company properties

Attribute | Type | Description
--------- | ---- | -----------
properties.name | string | The name of the company associated with this person
properties.domain | string | The domain of the company associated with this person
properties.location | object | The location of the company's headquarters.
properties.location.state | string | The headquarters' state name
properties.location.state_code | string | The headquarters' two-character state code
properties.location.country | string | The headquarters's country
properties.location.country_code | string | The headquarters's two-character country code
properties.location.tags | array | An array of tags describing the location
properties.number_of_employees | number | The number of employees at the company
properties.industry | string | The industry of the company

## Companies (with payload)

This API endpoint is similar to the one above but it lets you provide your own enriched company data.

<!-- ```shell
curl "https://api.madkudu.com/v1/companies?domain=madkudu.com" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

```javascript
madkudu.company.find({ domain: 'madkudu.com' })
    .then(function (company) {
        console.log(company);
    });
});
``` -->

```json
{
    "object_type": "company",
    "domain": "madkudu.com",
    "properties": {
        "name": "MadKudu",
        "domain": "madkudu.com",
        "location": {
            "state": "California",
            "state_code": "CA",
            "country": "United States",
            "country_code": "US",
            "tags": [
                "english_speaking",
                "high_gdp_per_capita"
            ]
        },
        "number_of_employees": 10,
        "industry": "Internet Software & Services",
        "customer_fit": {
            "segment": "good",
            "score": 37,
            "top_signals": [
                {
                    "name": "employee count",
                    "value": 10,
                    "type": "negative"
                },
                {
                    "name": "based in a country where gdp per capita",
                    "value": "high",
                    "type": "positive"
                },
                {
                    "name": "capital raised",
                    "value": 1370000,
                    "type": "positive"
                },
                {
                    "name": "belongs to industry where revenue per employee",
                    "value": "high",
                    "type": "positive"
                },
                {
                    "name": "number of tech found on website",
                    "value": 10,
                    "type": "positive"
                },
                {
                    "name": "web traffic volume",
                    "value": "low",
                    "type": "negative"
                }
            ]
        }
    }
}
```

### HTTP Request

`POST https://api.madkudu.com/v1/companies`

### Query parameters

Parameter | Description
--------- | -----------
domain | **string (required)** | The domain name of the company you would like to retrieve
company | **object (optional)** | The clearbit company object

### Properties

See [Company properties](#company-properties)



## Persons

Our Person API lets you lookup a person profile via an email address. It returns the predictive customer fit of the lead (aka. demographics score), the top signals behind this score (ie. why is this lead a good fit or not), and some light demographics information (eg. type of email, spam detection, number of employees, industry, etc.).


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
  "object_type": "person",
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
      "score": 200,
      "top_signals": [
        { "name": "Employees count", "value": "200", "type": "positive"},
        { "name": "Software industry", "value": true, "type": "positive"}
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

### Person properties

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

See [Company properties](#company-properties)

## Persons (with payload)

This API endpoint is similar to the one above but it lets you provide your own enriched person data.

<!-- ```shell
curl "https://api.madkudu.com/v1/persons?email=paul@madkudu.com" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

```javascript
madkudu.person.find({ email: 'paul@madkudu.com' })
    .then(function (person) {
        console.log(person);
    });
});
``` -->

```json
{
  "object_type": "person",
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
      "score": 200,
      "top_signals": [
        { "name": "Employees count", "value": "200", "type": "positive"},
        { "name": "Software industry", "value": true, "type": "positive"}
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

`POST https://api.madkudu.com/v1/persons`

### Query parameters

Parameter | Description
--------- | -----------
email | **string (required)** | The email of the lead you would like to retrieve
person | **object (optional)** | The clearbit person object
company | **object (optional)** | The clearbit company object

### Properties

See [Person properties](#person-properties)

# Models API
Our Models API takes the model ID, and either returns the configuration value or make changes to the model configuration. It is a good method to manage MadKudu's different prediction models in a way that would help sales and marketing teams increase conversions.

### Model Lookup
You can obtain a list of available models that have already been set up on the platform. To use the Models API, simply input the right request.

```shell
Key in your MadKudu API key under Authorization:

curl "https://api.madkudu.com/v1/models" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```
```json
{
  "results": {
      "models": [
          {
              "id": "123456",
              "Name": "find_industry",
              "type": "topical"
          },
          {
              "id": "143265",
              "name": "find_use_case",
              "type": "topical"
          }
      ]
  }
}
```

#### HTTP Request

`GET https://api.madkudu.com/v1/models`

#### Query Parameters
The following parameters are supported.

Parameter | Type
--------- | -----
id | **string (optional)**
name | **string (optional)**

## Topical Scorer Model
The Models API for Topical Scorer model lets you  extract insightful information from web properties of your leads. It returns the main topic most relevant to your leads with its prediction scores against your core use cases, industries and qualified needs.

### Configuration View
Sometimes you'll want to look up the configuration value of a specific Topical Scorer model directly through the API because you are making edits on the fly. The Models API returns the details of the model configuration such as topics, keywords and weights.

```json
{
    "id": "123",
    "name": "find_industry",
    "topics": [{
        "topic": "is_retailer",
        "keywords": [{
            "phrase": "store",
            "weight": 0.25
        }, {
            "phrase": "stores",
            "weight": 0.25
        }]
     }, {
        "topic": "is_restaurant",
        "keywords": [{
            "phrase": "menu",
            "weight": 1
        }, {
            "phrase": "nutrition",
            "weight": 1
        }]
    }]
}
```
#### HTTP Request

`GET https://api.madkudu.com/v1/models/:id`

#### Query Parameters
The following parameters are supported.

Parameter | Type
--------- | -----
id | **string (required)**
name | **string (optional)**

### Configuration of Model
To create a new Topical Scorer model, use POST with the configurations made from platform itself so send us the parameters that should be included in the configuration such as name and topics in the request body.

For each topic created, you can input different keywords with weights. In this case, weight directly impacts the importance of the keyword in the model. If the keyword appears on the lead's website page two times, that will be counted as two "hits". We will then calculate the weighted score for each topic based on: keywords_n(number of hits x weight). Then, we will then compute the overall score for each topic based on: sum of weighted scores for each keyword.

```json
{
    "id": "123",
    "name": "find_industry",
    "topics": [{
        "topic": "is_retailer",
        "keywords": [{
            "phrase": "store",
            "weight": 0.25
        }, {
            "phrase": "stores",
            "weight": 0.25
        }]
     }, {
        "topic": "is_restaurant",
        "keywords": [{
            "phrase": "menu",
            "weight": 1
        }, {
            "phrase": "nutrition",
            "weight": 1
        }]
    }]
}
```
#### HTTP Request

`POST https://api.madkudu.com/v1/models/`

#### Query Parameters
The following parameters are supported.

Parameter | Type
--------- | -----
name | **string (optional)**
topics | **string (optional)**

### Model Update
Sometimes you'll need to update your model with more relevant keywords or accurate weightage allocation. This update will be done on a specific model. Simply send us the model id and it will directly update the model with user-specified configurations.

#### HTTP Request

`POST https://api.madkudu.com/v1/models/:id`

#### Query Parameters
The following parameters are supported.

Parameter | Type
--------- | -----
id | **string (required)**

# Predictions API
The Predictions API takes a model id and provides predictive recommendations according to the model type.

## Topical Scorer Model
The Predictions API for Topical Scorer models takes a model id and domain name, and returns the topics found on the lead's website with the scores from highest to lowest. This is especially useful for de-anomyzing traffic on your website and customizing outreach efforts.

### Predictions of Stored Model
To obtain predictions of a stored model for a specific domain, send us the id and the domain in request body.

```json
{
    "domain": "patagonia.com.au",
    "topics": [
        {
            "name": "has_store",
            "hits": [
                {
                    "phrase": "store",
                    "hits": 5,
                    "weighted_score": 1.25
                },
                {
                    "phrase": "branch",
                    "hits": 1,
                    "weighted_score": 1
                },
            ],
            "score": 12.25
        },
        {
            "name": "has_stockist",
            "score": 0
        }
    ]
}
```
#### HTTP Request

`POST https://api.madkudu.com/v1/models/:id/predictions`

#### Query Parameters
The following parameters are supported.

Parameter | Type
--------- | -----
id | **string (required)**

### On-the-Fly Predictions
The common use case here is when creating a new Topical Scorer model, you may sometimes need to be able to get results on the fly while creating the model. For this, you do not have to send us the model id.

#### HTTP Request

`POST https://api.madkudu.com/v1/predictions`

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

<!-- ## Segment

This endpoint lets you send us data through HTTP

### HTTP Request

`POST https://data.madkudu.com/v1/segment`

The body of the request should follow the [segment spec](https://segment.com/docs/spec/).

We support the following event types:

- identify
- track
- page
- group
- alias
- screen

You can see examples for the `identify` and `payload` on the right. If you have any questions, and are not sure how to map your data to that spec, we'll be happy to assist you!

> identify payload:

```json
{
  "type": "identify",
  "traits": {
    "name": "Peter Gibbons",
    "email": "peter@initech.com",
    "plan": "premium",
    "logins": 5
  },
  "userId": "97980cfea0067"
}
```

> track payload

```json
{
  "type": "track",
  "event": "Registered",
  "properties": {
    "plan": "Pro Annual",
    "accountType" : "Facebook"
  },
  "userId": "97980cfea0067"
}
``` -->
