---
title: API | MadKudu

language_tabs:
  - shell: Shell
  - javascript: Node.js

toc_footers:
  - <a href='https://app.madkudu.com/signup'>Sign up for an API Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the MadKudu API.

- All requests should be made using _https://_.
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
const madkudu = require("@madkudu/madkudu-node")("your_api_key");
```

The MadKudu API uses HTTP Basic Auth and requires using HTTPS on all API calls.

Your API Key should be used as the basic auth username. You do not need to provide a password.

For example, if your user’s API key was `ABCD1234`, you need to Base64 encode the string `ABCD1234:` (Please note the colon at the end) and prepend the string `Basic ``. In this case, this would result in a final header of:

`Authorization: Basic QUJDRDEyMzQ6`

<aside class="notice">
The rest of this documentation assumes that you pass the authorization header with every call
</aside>

# Rate limiting

You can make up to 600 requests per endpoint per minute.
Once you reach that limit, you will start receiving errors (HTTP code 429).

# Demographics API

## Companies

Our Company API lets you lookup a company profile via a domain name. It returns the predictive customer fit of the company (aka. demographics score), the top signals behind this score (ie. why is this company a good fit or not), and some light demographics information (eg. number of employees, industry, etc.).

```shell
curl "https://api.madkudu.com/v1/companies?domain=madkudu.com" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

```javascript
const company = await madkudu.company.find({ domain: 'madkudu.com' })
console.log(company);
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
      "tags": ["english_speaking", "high_gdp_per_capita"]
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
      ],
      "top_signals_formated": "↑ based in a country where gdp per capita is high\n↑ capital raised is 1,370,000\n↑ belongs to industry where revenue per employee is high\n↑ number of tech found on website is 10\n✖ employee count is 10\n✖ web traffic volume is low"
    },
    "predicted_value": 327.4
  }
}
```

### HTTP Request

`GET https://api.madkudu.com/v1/companies?domain=:domain`

### Query Parameters

| Parameter | Description           |
| --------- | --------------------- | --------------------------------------------------------- |
| domain    | **string (required)** | The domain name of the company you would like to retrieve |

### Company properties

| Attribute                                     | Type   | Description                                                                                                                                                                            |
| --------------------------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| properties.name                               | string | The name of the company associated with this person                                                                                                                                    |
| properties.domain                             | string | The domain of the company associated with this person                                                                                                                                  |
| properties.location                           | object | The location of the company's headquarters                                                                                                                                             |
| properties.location.state                     | string | The headquarters' state name                                                                                                                                                           |
| properties.location.state_code                | string | The headquarters' two-character state code                                                                                                                                             |
| properties.location.country                   | string | The headquarters's country                                                                                                                                                             |
| properties.location.country_code              | string | The headquarters's two-character country code                                                                                                                                          |
| properties.location.tags                      | array  | An array of tags describing the location                                                                                                                                               |
| properties.number_of_employees                | number | The number of employees at the company                                                                                                                                                 |
| properties.industry                           | string | The industry of the company                                                                                                                                                            |
| properties.customer_fit                       | object | The standard MadKudu customer fit fields                                                                                                                                               |
| properties.customer_fit.segment               | string | The standard MadKudu Customer Fit segment which is either "low", "medium", "good" or "very good"                                                                                       |
| propoerties.customer_fit.score                | number | The standard MadKudu Customer Fit score which ranges from 0 to 100                                                                                                                     |
| properties.customer_fit.top_signals           | array  | The standard MadKudu Customer Fit signals presented in array format where each signal line is shown in name, value and type                                                            |
| properties.customer_fit.top_signals_formatted | string | The standard MadKudu Customer Fit signals presented all in one string                                                                                                                  |
| properties.predicted_value                    | number | (Optional) The value of an account before they have reached the end of the funnel (aka made a purchase), based on the historical value of similar accounts who have made the purchase. |

## Companies (with payload)

This API endpoint is similar to the one above but it lets you provide your own enriched company data.

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
      "tags": ["english_speaking", "high_gdp_per_capita"]
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
      ],
      "top_signals_formated": "↑ based in a country where gdp per capita is high\n↑ capital raised is 1,370,000\n↑ belongs to industry where revenue per employee is high\n↑ number of tech found on website is 10\n✖ employee count is 10\n✖ web traffic volume is low"
    },
    "predicted_value": 327.4
  }
}
```

### HTTP Request

`POST https://api.madkudu.com/v1/companies`

### Query Parameters

| Parameter | Description           |
| --------- | --------------------- | --------------------------------------------------------- |
| domain    | **string (required)** | The domain name of the company you would like to retrieve |
| company   | **object (required)** | The clearbit company object                               |

### Properties

See [Company properties](#company-properties)

## Persons

Our Person API lets you lookup a person profile via an email address. It returns the predictive customer fit of the lead (aka. demographics score), the top signals behind this score (ie. why is this lead a good fit or not), and some light demographics information (eg. type of email, spam detection, number of employees, industry, etc.).

```shell
curl "https://api.madkudu.com/v1/persons?email=paul@madkudu.com" \
  -H "Authorization: Basic QUJDRDEyMzQ6"
```

```javascript
const person = await madkudu.person.find({ email: 'paul@madkudu.com' });
console.log(person);
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
      ],
      "top_signals_formated": "↑ based in a country where gdp per capita is high\n↑ capital raised is 1,370,000\n↑ belongs to industry where revenue per employee is high\n↑ number of tech found on website is 10\n✖ employee count is 10\n✖ web traffic volume is low"
    },
    "predicted_value": 327.4
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
      "industry": "Software",
      "predicted_value": 327.4
    }
  }
}
```

### HTTP Request

`GET https://api.madkudu.com/v1/persons?email=:email`

### Query Parameters

| Parameter | Description           |
| --------- | --------------------- | ------------------------------------------------ |
| email     | **string (required)** | The email of the lead you would like to retrieve |

### Person properties

| Attribute                                     | Type    | Description                                                                                                                                                                     |
| --------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| properties.first_name                         | string  | First name of the person                                                                                                                                                        |
| properties.last_name                          | string  | Last name of the the person                                                                                                                                                     |
| properties.domain                             | string  | The domain associated with this email address                                                                                                                                   |
| properties.is_student                         | boolean | true if the email is identified as a student email                                                                                                                              |
| properties.is_spam                            | boolean | true if the email is identified as a spam email                                                                                                                                 |
| properties.is_personal_email                  | boolean | true if the email is identified as a personal (or disposable) email address (e.g. gmail.com)                                                                                    |
| properties.customer_fit                       | object  | The standard MadKudu customer fit fields                                                                                                                                        |
| properties.customer_fit.segment               | string  | The standard MadKudu Customer Fit segment which is either "low", "medium", "good" or "very good"                                                                                |
| propoerties.customer_fit.score                | number  | The standard MadKudu Customer Fit score which ranges from 0 to 100                                                                                                              |
| properties.customer_fit.top_signals           | array   | The standard MadKudu Customer Fit signals presented in array format where each signal line is shown in name, value and type                                                     |
| properties.customer_fit.top_signals_formatted | string  | The standard MadKudu Customer Fit signals presented all in one string                                                                                                           |
| properties.predicted_value                    | number  | (Optional) The value of a lead before they have reached the end of the funnel (aka made a purchase), based on the historical value of similar leads who have made the purchase. |

### Company properties

See [Company properties](#company-properties)

## Persons (with payload)

This API endpoint is similar to the one above but it lets you provide your own enriched person data.

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
      ],
      "top_signals_formated": "↑ based in a country where gdp per capita is high\n↑ capital raised is 1,370,000\n↑ belongs to industry where revenue per employee is high\n↑ number of tech found on website is 10\n✖ employee count is 10\n✖ web traffic volume is low"
    },
    "predicted_value": 327.4
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
      "industry": "Software",
      "predicted_value": 327.4
    }
  }
}
```

### HTTP Request

`POST https://api.madkudu.com/v1/persons`

### Query Parameters

| Parameter | Description           |
| --------- | --------------------- | ------------------------------------------------ |
| email     | **string (required)** | The email of the lead you would like to retrieve |
| person    | **object (required)** | The clearbit person object                       |
| company   | **object (required)** | The clearbit company object                      |

### Properties

See [Person properties](#person-properties)

# Job changes API

## Set Watch List

You can define with this endpoint which person do you want to track with the job changes features.
Posting a new watch list will replace the previous one (if existing).

```shell
curl "https://api.madkudu.com/v1/integrations/job_changes/watch-list/csv" \
  -H "Authorization: Basic QUJDRDEyMzQ6" \
  --form 'watch_list_csv=@"/bruce/files/watch-list-2024.csv"'
```

```csv
// example of csv file

email
nick.cage@cage.com
bruce.willis@willis.com
```

### HTTP Request

`POST https://api.madkudu.com//v1/integrations/job_changes/watch-list/csv`

### Form Data

| Key            | Type                | Description                                                                                        |
| -------------- | ------------------- | -------------------------------------------------------------------------------------------------- |
| watch_list_csv | **file (required)** | The csv file containing your watchlist, it should include at least one column containing the email |

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
