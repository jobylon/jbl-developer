---
title: Push API and Webhooks

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='mailto:info@jobylon.com?subject=PushAPI-credentials'>Email us to get your credentials</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---


# Push API

API for our integration partners, who want to push candidates into Jobylon.

## Model

In Jobylon every candidate can be represented by one or more
applications, where every application relates to a job.

## Workflow

The 3:rd party app registers with Jobylon and gets one or more
sets of credentials plus one or more feeds, that contains the promotions or
jobs for one or more companies. The app uses the credentials and a job_id
(found in the feed) to push new applications to Jobylon.

The type of feed (promotion or job) depends on the type of data required by the
app and how our clients' contract work with the 3:rd party. If the 3:rd party
do not require any extra information (such as purchase information) a simple
job feed will be provided, otherwise a promotion feed will be provided. The
promotion feed wraps a job and contains extra information.

## Application

> To run the examples:

```
export HOST='https://staging.jobylon.com'
export API_VERSION='p1'
export APP_ID='0123456789123456'
export APP_KEY='AbC123XyZ'
```

> Basic example:

```shell
# Request
curl -i \
    -X POST "$HOST/$API_VERSION/applications/" \
    -H "X-App-Id: $APP_ID" \
    -H "X-App-Key: $APP_KEY" \
    -H "Content-Type: application/json" \
    -d '{
        "job_id": 123456789,
        "first_name": "Kalle",
        "last_name": "Kula",
        "email": "kalle@kula.se",
        "phone": "+4670-123456789",
        "ln_url": "https://www.linkedin.com/in/kalle-kula-123a4567",
        "message": "Message from the applicant...",
        "source_type": "applied",
        "source_json": {
            "partner_name": "best-source",
            "message": "Some other message...",
            "referrer": {
                "name": "Kella Kalu",
                "email": "kello@kalu.se",
                "phone": "+46123456789",
                "avatar": "https://gravatar.com/avatar/ce757a5d51e6285434134e7b6c96ab86?s=200&d=robohash&r=g"
            }
            "questions": [
                {
                    "order": 1,
                    "question": "Why should we hire the person?",
                    "question_type": "text",
                    "answer": "Because she is great!"
                }, {
                    "order": 2,
                    "question": "Rank the skills",
                    "question_type": "range",
                    "question_args": {
                        "min": 1,
                        "max": 5,
                        "step": 1,
                        "unit": "star"
                    },
                    "answer": 4
                }, {
                    "order": 3,
                    "question": "Where can they be located?",
                    "question_type": "select-multiple",
                    "question_alternatives": [
                        "Avesta",
                        "London",
                        "Moskva",
                        "New York",
                        "Paris",
                        "Stockholm"
                    ],
                    "answer": [
                        "London",
                        "New York",
                        "Paris",
                        "Stockholm"
                    ]
                }, {
                  "order": 4,
                  "question": "Do they have a EU work permit?", 
                  "question_type": "select-one",
                  "answer": "yes"
                }
            ]
        }
        "original_referrer": "https://bestreferrals.com/?utm_source=google&utm_medium=cpc&utm_term=earn_referrals",
        "ab_test": 'ABTestId'
    }'
```

```shell
# Response
Status Code: 201 Created
Content-Type: application/json

{'id': 123}
```

> Example using a local JSON file

```shell

# Request
curl -i \
    -X POST "$HOST/$API_VERSION/applications/" \
    -H "X-App-Id: $APP_ID" \
    -H "X-App-Key: $APP_KEY" \
    -H "Content-Type: application/json"
    -d @<PATH-TO-FILE>
```

> Example with files attached (multipart/form-data):

```shell

# Request
curl -i \
    -X POST "$HOST/$API_VERSION/applications/" \
    -H "X-App-Id: $APP_ID" \
    -H "X-App-Key: $APP_KEY" \
    -F "job_id=55" \
    -F "first_name=Kalle" \
    -F "last_name=Kula" \
    -F "email=kalle@kula.se" \
    -F "phone=+4670-123456789" \
    -F "message=Message from the applicant..." \
    -F "source_type=applied" \
    -F "source_json={
        \"partner_name\": \"best-source\",
        \"message\": \"Some other message...\"
    }" \
    -F "cv=@cv.pdf" \
    -F "cover_letter=@cover_letter.pdf" \
    -F "other_1=@other_1.pdf" \
    -F "other_2=@other_2.pdf" \
    -F "other_3=@other_3.pdf" \
    -F "other_4=@other_4.pdf" \
    -F "other_5=@other_5.pdf"
```

### Create

POST /applications/

**Query String Parameters**

None

**Request Payload**

| Name               | Type    | Mandatory? | Description                                |
| ---                | ---     | ---        | ---                                        |
| job_id             | integer | yes        | Applicant first name                       |
| first_name         | string  | yes        | Applicant first name                       |
| last_name          | string  | yes        | Applicant last name                        |
| email              | string  |            | Applicant email                            |
| phone              | string  |            | Applicant phone                            |
| ln_url             | URL     |            | Applicant LinkedIn URL (will be validated) |
| message            | string  |            | Message from the applicant                 |
| source_type        | string  | yes        | Source type (applied/applied-silent/recommended/sourced). If set to applied, a thank-you email will be sent to the applicant. |
| source_json        | object  | yes        | Additional source data (partner dependent, but using the data from the example will be nicely styled in Jobylon. |
| cv                 | file    |            | Application file (supported using multipart/form-data)  |
| cover_letter       | file    |            | Application file (supported using multipart/form-data)  |
| other_1            | file    |            | Application file (supported using multipart/form-data)  |
| other_2            | file    |            | Application file (supported using multipart/form-data)  |
| other_3            | file    |            | Application file (supported using multipart/form-data)  |
| other_4            | file    |            | Application file (supported using multipart/form-data)  |
| other_5            | file    |            | Application file (supported using multipart/form-data)  |
| ab_test            | string  |            | A unique identifier used for A/B testing                |
| original_referrer  | string  |            | Value used to keep track on the application origin (used in analytics) |


**Response**

| Name | Type    | Description    |
| ---  | ---     | ---            |
| id   | integer | Application ID |

**Exceptions**

| Status | Description                                          |
| ---    | ---                                                  |
| 400    | Bad request, job_id that app doesn't have access to. |
| 403    | Permission denied                                    |
| 405    | Method not supported                                 |

# Webhooks

Callback API for our intergration partners who want to receive and act upon
different Jobylon events.

## Model

In Jobylon every webhook has a client url and an event type that it is
subscribed to.

## Workflow

After the third-party party app registers with Jobylon; they can request
webhook intergration by providing a url and the type of events it should
subscribe to. The webhook will use these details to send notifications when an
event occurs.

When the event is sent to the url the webhook expects a successful response
(HTTP 2XX).  In the case it does not receive a valid response, it will retry
again immediately and then after 10 seconds, 100 seconds, 1000 seconds (~17
minutes), 10000 seconds (~2.8 hours) and 100000 seconds (~a day and three
hours).

## Headers

You can request us to provide you with custom headers if you so choose.

## Authentication

You can request us to provide `basic authentication` by providing a username and password of your choosing.

## Limit IP

You can limit the IPs that the callback comes from by letting us know.

## Event Types

i. [Application Event](#applicationevent)
ii. [Job Event](#jobevent)

### ApplicationEvent

> Example:

```
# Note this example is for an application event
# Request
Request Method: POST
Content-Type: application/json

{
  "event_type": "application",
  "action": "status_changed",
  "application": {
    "id": 1,
    "first_name": "Kalle",
    "last_name": "Kula",
    "email": "kalle@kula.se",
    "source_type": "applied",
    "status": {
      "id": 2,
      "name": "In Progress",
      "group": 1
    },
    "owner": {
      "id": 1,
      "name": "Application Owner",
      "email": "application.owner@company.com"
    },
    "rejection_sent": false,
    "ab_test": "ab-test-1",
    "job": {
      "id": 1,
      "title": "Some title",
      "from_date": "2018-01-01T00:00:00.001Z",
      "to_date": null,
      "status": 1,
      "contact_name": "Manager's Name",
      "contact_email": "manager@company.com",
      "employment_type": {
        "id": 1,
        "text": "Full-time"
      },
      "experience": {
        "id": 1,
        "text": "Entry level"
      },
      "function": {
        "id": 1,
        "text": "IT & Infrastructure"
      },
      "language": "sv",
      "location_set": [
        {
          "location": "Stockholm",
          "location_json": {
            "country_short": "SE", 
            "city": "Stockholm", 
            "url": "https://maps.google.com/?q=Stockholm,+Sweden&ftid=0x465f763119640bcb:0xa80d27d3679d7766", 
            "country": "Sweden", 
            "place_id": "ChIJywtkGTF2X0YRZnedZ9MnDag", 
            "area_1_short": "Stockholm County", 
            "area_1": "Stockholm County", 
            "city_short": "Stockholm"
          }
        }
      ],
      "categories": [
        {
          "id": 1,
          "text": "Technology"
        }
      ],
      "departments": [
        {
          "id": 1,
          "text": "IT"
        }
      ],
      "company": {
        "id": 1,
        "name": "Some Company"
      },
      "owner": {
        "id": 2,
        "name": "Job Owner",
        "email": "job.owner@company.com"
      }
    }
  }
}
```

```
# Response
Status Code: 2XX
```

**Actions**

| Name           | Description                              |
| ---            | ---                                      |
| created        | Application created                      |
| rejection_sent | Rejection communicated/sent to applicant |
| status_changed | Application status updated               |

**Request Payload**

| Name        | Type   | Description                               |
| ---         | ---    | ---                                       |
| event_type  | string | "application"                             |
| action      | string | Action triggering the event (see above)   |
| application | object | The application object  |

### JobEvent

**Actions**

| Name           | Description                         |
| ---            | ---                                 |
| created        | Job created                         |
| updated        | Job was updated                     |
| status_changed | Job status was specifically updated |

**Request Payload**

| Name       | Type   | Description                             |
| ---        | ---    | ---                                     |
| event_type | string | "job"                                   |
| action     | string | Action triggering the event (see above) |
| job        | object | The [job](#job) object                  |

### Application

| Name           | Type    | Description                                                                   |
| ---            | ---     | ---                                                                           |
| id             | integer | Application ID                                                                |
| first_name     | string  | First name                                                                    |
| last_name      | string  | Last name                                                                     |
| email          | string  | Email                                                                         |
| rejection_sent | boolean | Info regarding if the rejection has been communicated to the applicant or not |
| status         | object  | The [status](#status) object                                                  |
| source_type    | string  | The [source](#source-type) where the application was received                 |
| job            | object  | The [job](#job) object                                                        |
| owner          | object  | The [user](#user) that owns the application                                   |
| ab_test        | string  | A unique identifier used for A/B testing                                      |

### Source Type

| Value            | Description                                                        |
| ---              | ---                                                                |
| 'applied'        | Applied (thank-you email sent by Jobylon)                          |
| 'applied-silent' | Applied through partner (thank-you email possibly sent by partner) |
| 'imported'       | Imported                                                           |
| 'recommended'    | Recommended                                                        |
| 'sourced'        | Sourced                                                            |
| 'uploaded'       | Uploaded                                                           |

### Status

| Name  | Type    | Description                             |
| ---   | ---     | ---                                     |
| id    | integer | Status ID                               |
| name  | string  | Status name                             |
| group | integer | The [status group](#status-group) value |

### Status Group

| Value | Description |
| ---   | ---         |
| 0     | New         |
| 1     | In progress |
| 19    | Rejected    |
| 20    | Hired       |
| 21    | On hold     |

### Job

| Name            | Type                   | Description                                             |
| ---             | ---                    | ---                                                     |
| id              | integer                | Job ID                                                  |
| title           | string                 | Title of the job                                        |
| from_date       | string (date-time UTC) | Datetime job first created                              |
| to_date         | string (date-time UTC) | Deadline for the job                                    |
| contact_name    | string                 | Contact name                                            |
| contact_email   | string                 | Contact email                                           |
| language        | string                 | The [language](#language) of the job                    |
| location_set    | array                  | The [location(s)](#location) of the job                 |
| categories      | array                  | The [categories](#category) that this job belongs to    |
| departments     | array                  | The [departments](#department) that this job belongs to |
| status          | string                 | The [job status](#job-status) value                     |
| company         | object                 | The [company](#company) object                          |
| owner           | object                 | The [user](#user) that owns the job                     |
| employment_type | object                 | The [employment type](#employtment-type) object         |
| experience      | object                 | The [experience](#experience) object                    |
| function        | object                 | The [function](#function) object                        |

### Job Status

| Value       | Description |
| ---         | ---         |
| 'draft'     | Draft       |
| 'published' | Published   |
| 'closed'    | Closed      |
| 'archived'  | Archived    |

### Category

| Name    | Type    | Description          |
| ---     | ---     | ---                  |
| id      | integer | Category ID          |
| text    | string  | Category description |

### Company

| Name  | Type    | Description  |
| ---   | ---     | ---          |
| id    | integer | Company ID   |
| name  | string  | Name         |

### Department

| Name    | Type    | Description     |
| ---     | ---     | ---             |
| id      | integer | Department ID   |
| name    | string  | Department name |

### Experience

| Name    | Type    | Description            |
| ---     | ---     | ---                    |
| id      | integer | Experience ID          |
| text    | string  | Experience description |

### Employment Type

| Name    | Type    | Description            |
| ---     | ---     | ---                    |
| id      | integer | EmploymentType ID          |
| text    | string  | EmploymentType description |

### Function

| Name    | Type    | Description          |
| ---     | ---     | ---                  |
| id      | integer | Function ID          |
| text    | string  | Function description |

### Language

| Value | Description |
| ---   | ---         |
| 'da'  | Dansk       |
| 'de'  | Deutsch     |
| 'en'  | English     |
| 'fi'  | Suomi       |
| 'fr'  | Français    |
| 'nb'  | Norsk       |
| 'nl'  | Nederlands  |
| 'sv'  | Svenska     |

### Location

| Name          | Type   | Description                                    |
| ---           | ---    | ---                                            |
| location      | string | name of the location                           |
| location_json | object | object based on the data from Google's map API |

### User

| Name  | Type    | Description  |
| ---   | ---     | ---          |
| id    | integer | User ID      |
| name  | string  | Name         |
| email | string  | Email        |

## A/B Testing

A/B testing for customers who want to try different application flows

### Through Jobylon

You can pass the name of the A/B test as a query parameter on the Jobylon URL. The query parameter should be called `jbl_ab_test`. We will store it in the browser session storage for 30 minutes or until the session expires, whichever is shorter. In other words, it will be stored for a maximum of 30 minutes. If a new A/B testing query parameter is found on the URL, the existing one will be overridden and the timeout will be reset to 30 minutes. As long as the user ends up on the application form within 30 minutes, the A/B testing query parameter value will be stored with the application. A `ab_test` parameter is added to the application data sent to your [webhook](#webhooks).

### Through the Push API

Just add the `ab_test` parameter when pushing to Jobylon and the data will be stored in Jobylon.

<aside class="notice">
Please ensure that the <code>ab_test</code> parameter is less than <code>20 characters</code>
</aside>


### Usecase example

[![](https://mermaid.ink/img/eyJjb2RlIjoic2VxdWVuY2VEaWFncmFtXG4gICAgcGFydGljaXBhbnQgQyBhcyBDYW5kaWRhdGVcbiAgICBwYXJ0aWNpcGFudCBXIGFzIFdlYlxuICAgIHBhcnRpY2lwYW50IEogYXMgSm9ieWxvblxuICAgIHBhcnRpY2lwYW50IEJJIGFzIEJJXG4gICAgQy0-Plc6IEZvbGxvdyBsaW5rIHRvIHNpdGVcbiAgICBhY3RpdmF0ZSBXXG4gICAgTm90ZSByaWdodCBvZiBDOiBTZXNzaW9uIGdldHMgdGFnZ2VkPGJyLz4gd2l0aCBhIHVuaXF1ZSBpZC48YnIvPiBFeDogWFlaMTIzXG4gICAgVy0-Pko6IEZvbGxvdyBsaW5rIHRvIEpvYnlsb25cbiAgICBkZWFjdGl2YXRlIFdcbiAgICBhY3RpdmF0ZSBKXG4gICAgTm90ZSByaWdodCBvZiBXOiBMaW5rIGNvbnRhaW5zIHF1ZXJ5PGJyLz4gcGFyYW1ldGVyIGFiX3Rlc3QuPGJyLz4gRXg6ID9hYl90ZXN0PVhZWjEyMyAgXG4gICAgSi0-Pko6IENhbmRpYXRlIGFwcGxpZXMgdG8gam9iXG4gICAgYWx0IEJJIERCIGFjY2Vzc1xuICAgICAgICBOb3RlIHJpZ2h0IG9mIEo6IE5pZ2h0bHkgZXhwb3J0IHRvPGJyLz4gZGVkaWNhdGVkIGRhdGFiYXNlPGJyLz4gaG9zdGVkIGJ5IEpvYnlsb248YnIvPiBhbmQgYWNjZXNzZWQgYnk8YnIvPiBCSSB0b29sLiBcbiAgICBlbHNlIENTViBleHBvcnRcbiAgICAgICAgTm90ZSByaWdodCBvZiBKOiBNYW51YWwgZXhwb3J0ZWQgYnk8YnIvPiBKb2J5bG9uIFN1cHBvcnQ8YnIvPiBpbXBvcnRlZCBpbnRvIEJJPGJyLz4gYnkgY2xpZW50LlxuICAgIGVsc2UgV2ViaG9va3NcbiAgICAgICAgSi0-PkJJOiBBcHBsaWNhdGlvbkV2ZW50OmNyZWF0ZWRcbiAgICAgICAgYWN0aXZhdGUgQklcbiAgICAgICAgTm90ZSByaWdodCBvZiBKOiBBbGwgYXBwbGljYXRpb24gZGF0YTxici8-IGluYy4gaWQsIGFiX3Rlc3QuLi5cbiAgICAgICAgQkktPj5CSTogUHJvY2VzcyBkYXRhIGFuZCBjb25uZWN0PGJyLz4gdG8gb3RoZXIgZGF0YSBzb3VyY2VzXG4gICAgICAgIEJJLS0-Pko6IFxuICAgICAgICBkZWFjdGl2YXRlIEJJXG4gICAgICAgIGRlYWN0aXZhdGUgSlxuICAgICAgICBsb29wIFN0YXR1cyBjaGFuZ2VkXG4gICAgICAgICAgICBhY3RpdmF0ZSBKXG4gICAgICAgICAgICBKLT4-Qkk6IEFwcGxpY2F0aW9uRXZlbnQ6c3RhdHVzX2NoYW5nZWRcbiAgICAgICAgICAgIGFjdGl2YXRlIEJJXG4gICAgICAgICAgICBOb3RlIHJpZ2h0IG9mIEo6IEFsbCBhcHBsaWNhdGlvbiBkYXRhPGJyLz4gaW5jLiBpZCwgYWJfdGVzdC4uLlxuICAgICAgICAgICAgQkktPj5CSTogUHJvY2VzcyBkYXRhIGFuZCBhZGQ8YnIvPiBzdGF0dXMgY2hhbmdlc1xuICAgICAgICAgICAgQkktLT4-SjogXG4gICAgICAgICAgICBkZWFjdGl2YXRlIEJJXG4gICAgICAgICAgICBkZWFjdGl2YXRlIEpcbiAgICAgICAgZW5kXG4gICAgZW5kIiwibWVybWFpZCI6eyJ0aGVtZSI6ImRlZmF1bHQifX0)](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoic2VxdWVuY2VEaWFncmFtXG4gICAgcGFydGljaXBhbnQgQyBhcyBDYW5kaWRhdGVcbiAgICBwYXJ0aWNpcGFudCBXIGFzIFdlYlxuICAgIHBhcnRpY2lwYW50IEogYXMgSm9ieWxvblxuICAgIHBhcnRpY2lwYW50IEJJIGFzIEJJXG4gICAgQy0-Plc6IEZvbGxvdyBsaW5rIHRvIHNpdGVcbiAgICBhY3RpdmF0ZSBXXG4gICAgTm90ZSByaWdodCBvZiBDOiBTZXNzaW9uIGdldHMgdGFnZ2VkPGJyLz4gd2l0aCBhIHVuaXF1ZSBpZC48YnIvPiBFeDogWFlaMTIzXG4gICAgVy0-Pko6IEZvbGxvdyBsaW5rIHRvIEpvYnlsb25cbiAgICBkZWFjdGl2YXRlIFdcbiAgICBhY3RpdmF0ZSBKXG4gICAgTm90ZSByaWdodCBvZiBXOiBMaW5rIGNvbnRhaW5zIHF1ZXJ5PGJyLz4gcGFyYW1ldGVyIGFiX3Rlc3QuPGJyLz4gRXg6ID9hYl90ZXN0PVhZWjEyMyAgXG4gICAgSi0-Pko6IENhbmRpYXRlIGFwcGxpZXMgdG8gam9iXG4gICAgYWx0IEJJIERCIGFjY2Vzc1xuICAgICAgICBOb3RlIHJpZ2h0IG9mIEo6IE5pZ2h0bHkgZXhwb3J0IHRvPGJyLz4gZGVkaWNhdGVkIGRhdGFiYXNlPGJyLz4gaG9zdGVkIGJ5IEpvYnlsb248YnIvPiBhbmQgYWNjZXNzZWQgYnk8YnIvPiBCSSB0b29sLiBcbiAgICBlbHNlIENTViBleHBvcnRcbiAgICAgICAgTm90ZSByaWdodCBvZiBKOiBNYW51YWwgZXhwb3J0ZWQgYnk8YnIvPiBKb2J5bG9uIFN1cHBvcnQ8YnIvPiBpbXBvcnRlZCBpbnRvIEJJPGJyLz4gYnkgY2xpZW50LlxuICAgIGVsc2UgV2ViaG9va3NcbiAgICAgICAgSi0-PkJJOiBBcHBsaWNhdGlvbkV2ZW50OmNyZWF0ZWRcbiAgICAgICAgYWN0aXZhdGUgQklcbiAgICAgICAgTm90ZSByaWdodCBvZiBKOiBBbGwgYXBwbGljYXRpb24gZGF0YTxici8-IGluYy4gaWQsIGFiX3Rlc3QuLi5cbiAgICAgICAgQkktPj5CSTogUHJvY2VzcyBkYXRhIGFuZCBjb25uZWN0PGJyLz4gdG8gb3RoZXIgZGF0YSBzb3VyY2VzXG4gICAgICAgIEJJLS0-Pko6IFxuICAgICAgICBkZWFjdGl2YXRlIEJJXG4gICAgICAgIGRlYWN0aXZhdGUgSlxuICAgICAgICBsb29wIFN0YXR1cyBjaGFuZ2VkXG4gICAgICAgICAgICBhY3RpdmF0ZSBKXG4gICAgICAgICAgICBKLT4-Qkk6IEFwcGxpY2F0aW9uRXZlbnQ6c3RhdHVzX2NoYW5nZWRcbiAgICAgICAgICAgIGFjdGl2YXRlIEJJXG4gICAgICAgICAgICBOb3RlIHJpZ2h0IG9mIEo6IEFsbCBhcHBsaWNhdGlvbiBkYXRhPGJyLz4gaW5jLiBpZCwgYWJfdGVzdC4uLlxuICAgICAgICAgICAgQkktPj5CSTogUHJvY2VzcyBkYXRhIGFuZCBhZGQ8YnIvPiBzdGF0dXMgY2hhbmdlc1xuICAgICAgICAgICAgQkktLT4-SjogXG4gICAgICAgICAgICBkZWFjdGl2YXRlIEJJXG4gICAgICAgICAgICBkZWFjdGl2YXRlIEpcbiAgICAgICAgZW5kXG4gICAgZW5kIiwibWVybWFpZCI6eyJ0aGVtZSI6ImRlZmF1bHQifX0)
