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

### Query Parameters

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

### Query Parameters

Parameter | Description
--------- | -----------
email | **string (required)** | The email of the lead you would like to retrieve
person | **object (optional)** | The clearbit person object
company | **object (optional)** | The clearbit company object

### Properties

See [Person properties](#person-properties)

# Models API
Our Models API takes the model ID, and either returns the configuration value or make changes to the model configuration. It is a good method to manage MadKudu's different prediction models in a way that would help sales and marketing teams increase conversions.

## List Models
You can obtain a list of available models that have already been set up on the platform, and its configuration. To use the Models API, simply input the right request which, in this case, is just your API key.

```shell
Key in your MadKudu API key under Authorization:

curl "https://api.madkudu.com/v1/models" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

> The API response will look like this if you only have one model saved:

```json
{
    "id": "BJ_sP3gsG",
    "version": "2.0",
    "type": "topical",
    "name": "find_industry",
    "config": {
        "topics": [
                    {
                    "keywords": [
                        {
                            "weight": 1,
                            "phrase": "online"
                        }
                    ],
                    "name": "e-commerce"
                },
                {
                    "keywords": [
                        {
                            "weight": 1,
                            "phrase": "location"
                        }
                    ],
                    "name": "physical_store"
                }
            ],
        "confidence_mapping": [
            {
                "score": 0,
                "confidence": 0

            },
            {
                "score": 30,
                "confidence": 50
            },
            {
                "score": 60,
                "confidence": 100
            }
        ]
    }
}
```

### HTTP Request

`GET https://api.madkudu.com/v1/models`

## Get a Model Definition
Sometimes you'll want to look up the configuration value of a specific model that you've created and saved. The Models API returns the details of the model configuration such as keywords, phrases and weights as well as its settings for confidence_mapping.

> The API response will look like this:

```json
{
    "id": "BJ_sP3gsG",
    "version": "2.0",
    "type": "topical",
    "name": "find_industry",
    "config": {
        "topics": [
            {
                "keywords": [
                    {
                        "weight": 1,
                        "phrase": "online"
                    },
                    {
                        "weight": 0.5,
                        "phrase": "delivery"
                    }
                ],
                "name": "e-commerce"
            },
            {
                "keywords": [
                    {
                        "weight": 1,
                        "phrase": "location"
                    }
                ],
                "name": "physical_store"
            }
        ],
        "confidence_mapping": [
            {
                "score": 0,
                "confidence": 0
            },
            {
                "score": 30,
                "confidence": 50
            },
            {
                "score": 60,
                "confidence": 100
            }
        ]
    }
}
```

### HTTP Request

`GET https://api.madkudu.com/v1/models/:id`

### Query Parameters
The following parameters are supported in the URL query:

Parameter | Type | Description
--------- | ----- | -----------
id | **string (required)** | ID of the model you would like to see the definition for

## Configure a New Model
To create a new model, use POST with the configurations made from platform itself so send us the parameters that should be included in the configuration such as "type", "config" and "confidence_mapping" in the request body.

For each topic created, you can input different keywords with weights. In this case, weight directly impacts the importance of the keyword in the model. If the keyword appears on the lead's website page two times, that will be counted as two "counts" (see Predictions API). We will then calculate the weighted score for each topic based on: keyword_n(number of counts x weight). Then, we will compute the overall score for each topic based on: sum of weighted scores for each keyword. This score will be mapped to confidence using the settings you've configured in confidence_mapping. Confidence ranges from 0 to 100.

> Example of a request body:

```json
{
    "type": "topical",
    "name": "find_industry",
    "version": "2.0",
    "config": {
        "topics": [
                    {
                    "keywords": [
                        {
                            "weight": 1,
                            "phrase": "online"
                        },
                         {
                            "weight": 0.5,
                            "phrase": "delivery"
                        }
                    ],
                    "name": "e-commerce"
                },
                {
                    "keywords": [
                        {
                            "weight": 1,
                            "phrase": "location"
                        }
                    ],
                    "name": "physical_store"
                }
            ],
        "confidence_mapping": [
            {
                "score": 0,
                "confidence": 0

            },
            {
                "score": 30,
                "confidence": 50
            },
            {
                "score": 60,
                "confidence": 100
            }
        ]
    }
}
```

### HTTP Request

`POST https://api.madkudu.com/v1/models/`

### Body Parameters
The following parameters are supported in the request body:

Parameter | Type | Description
--------- | -----|-------------
type | **string (required)** | For topical models, type should be "topical"
name | **string (optional)** | Good to include a name for ease of identifying the models you created but if not included, there will not be a name object for the model created
version | **string (optional)** | If not included, it will default to 1.0
config | **object (required)** | Keyword names and phrases with their respective weights
confidence_mapping | **object (required)** | Required to obtain predictions in Predictions API

> The API response will look like this (the topics and confidence_mapping object will reflect what you've entered in the request body):

```json
{
    "id": "BJ_sP3gsG",
    "version": "2.0",
    "type": "topical",
    "name": "find_industry",
    "config": {
        "topics": [
        ],
        "confidence_mapping": [
        ]
    }
}
```

## Update a Model
Sometimes you'll need to update your model with more relevant keywords or accurate weightage allocation. This update will be done for a specific model by calling a required parameter: "id". Simply send us the model ID and it will directly update the model with the user-specified configurations.

> The metadata of the request body and API response will look the same as when you configure a new model

### HTTP Request

`PUT https://api.madkudu.com/v1/models/:id`

### Query Parameters
The following parameters are supported in the URL query:

Parameter | Type | Description
--------- | ----- | -----------
id | **string (required)** | ID of the model you would like to update

### Body Parameters
The following parameters are supported in the request body:

Parameter | Type | Description
--------- | -----|-------------
type | **string (required)** | For topical models, type should be "topical"
name | **string (optional)** | If not included, it will remain the same as what it is upon creation
version | **string (optional)** | If not included, it will remain the same as the version number upon creation
config | **object (required)** | Keyword names and phrases with their respective weights
confidence_mapping | **object (required)** | Required to obtain predictions in Predictions API

## Delete a Model
Sometimes you'll need to delete a model you've saved previously.

### HTTP Request

`DELETE https://api.madkudu.com/v1/models/:id`

### Query Parameters
The following parameters are supported in the URL query:

Parameter | Type | Description
--------- | ----- | ----------
id | **string (required)** | ID of the model you would like to delete

# Predictions API
The Predictions API takes a model ID and provides predictive recommendations according to the model type.

## Obtain Predictions for a Stored Model
To obtain predictions of a stored model for a specific domain, send us the "id", "domain" and "type" parameters in the request body. The default response will only show confidence. However, you can choose to see both confidence and score by adding "show_scores" parameter in the request body.

> The API response will look like this if there is a match on all topics:

```json
{
    "domain": "lazada.com",
    "main_topic": {
        "name": "e-commerce",
        "confidence": 7
    },
    "topics": [
        {
            "name": "e-commerce",
            "confidence": 7,
            "hits": [
                {
                    "phrase": "online",
                    "count": 1
                },
                {
                    "phrase": "delivery",
                    "count": 2
                }
            ]
        },
        {
            "name": "physical_store",
            "confidence": 0
        }
    ]
}
```

> The API response will look like this if there is no match on all topics:

```json
{
    "domain": "carlsjr.com",
    "topics": [
        {
            "name": "e-commerce",
            "confidence": 0
        },
        {
            "name": "physical_store",
            "confidence": 0
        }
    ]
}
```

### HTTP Request

`POST https://api.madkudu.com/v1/models/:id/predictions`

### Query Parameters
The following parameters are supported in the URL query:

Parameter | Type | Description
--------- | ----- | ------------
id | **string (required)** | ID of the model you would like to obtain predictions for

### Body Parameters
The following parameters are supported in the request body:

Parameter | Type | Description
--------- | ----- | ------------
domain | **string (required)** | Domain name of the lead you would like to obtain predictions for with the following convention: "domain.com"
show_scores | **string (optional)** | If you would like to show scores, enter "true" here, but if not included, show_scores functionality will be turned off by default

### Default Response for No-Match Leads
There are times when you enter a domain, and the domain does not match any of the topics you entered. This will automatically remove the "main_topic" and "hits" array in the API response since the "confidence" and "count" value is '0'.

## Obtain On-the-Fly Predictions
You may sometimes need to get results on the fly while creating the model. For this, you do not have to send us the model ID. You simply have to include the "type", "topics" and "confidence_mapping" parameters required for model configuration, as well as the domain you would like to obtain predictions for. The syntax of the request may be slightly different as seen in the example request body. Again, you can choose to see both confidence and score by adding the "show_scores" parameter in the request body.

> Example of a request body:

```json
{
  "domain": "patagonia.com.au",
  "type": "topical",
  "topics":[
    {
      "topic": "has_store",
      "keywords": [{
         "phrase": "store",
         "weight": 0.25
        }, {
         "phrase": "locate",
         "weight": 1
        }, {
         "phrase": "branch",
         "weight": 1
        }, {
          "weight": -1
        }]}, {
        "topic": "has_stockist",
        "keywords": [{
         "phrase": "stockist",
         "weight": 1
        }, {
         "phrase": "stockists",
         "weight": 1
        }, {
         "phrase": "distributor",
         "weight": 0.5
        }
      ]
    }
  ],
  "confidence_mapping": [
            {
                "score": 0,
                "confidence": 0

            },
            {
                "score": 30,
                "confidence": 50
            },
            {
                "score": 60,
                "confidence": 100
            }
        ]
}
```

### HTTP Request

`POST https://api.madkudu.com/v1/predictions`

### Body Parameters
The following parameters are supported in the request body:

Parameter | Type | Description
--------- | -----|-------------
domain | **string (required)** | Domain name of the lead you would like to obtain predictions for with the following convention: "domain.com"
type | **string (required)** | For topical models, type should be "topical"
topics | **object (required)** | Keyword names and phrases with their respective weights
confidence_mapping | **object (required)** | Required to obtain predictions in Predictions API
show_scores | **string (optional)** | If you would like to show scores, enter "true" here, but if not included, show_scores functionality will be turned off by default

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
