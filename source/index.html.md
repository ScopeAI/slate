---
title: ScopeAI API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://www.getscopeai.com/company/api_keys'>Sign Up for an API Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the ScopeAI API! You can use our API to access ScopeAI API endpoints, where you can pull all of your customer conversation data along with its associated metadata.

We currently don't get have any language bindings, but please drop us a note if having a specific language binding would be helpful. You can view request examples in the dark area to the right.

The code behind this API documentation page can be found on [github](https://github.com/scopeai/slate); please create an issue or pull request for any changes you'd like to see implemented. This API documentation page was created with [Slate](https://github.com/tripit/slate).

# Authentication



```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Token token=pLac3keYh3r3"
```


> Make sure to replace `pLac3keYh3r3` with your API key.

ScopeAI uses API keys as tokens to allow access to the API. You can register for new API key in the api page of your company found [here](https://wwww.getscopeai.com/company/api_keys).

ScopeAI expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Token token=pLac3keYh3r3`

<aside class="notice">
You must replace <code>pLac3keYh3r3</code> with your personal API key.
</aside>

# Pagination

> Example paging header

```
Link: <https://api.getscopeai.com/v1/tickets?page=2&per_page=25>; rel="next",
<https://api.getscopeai.com/v1/tickets?page=474&per_page=25>; rel="last"
```

API methods that return a collection of results are always paginated. Paginated results will include a `Link` (see [RFC-5988](https://tools.ietf.org/html/rfc5988)) response header with the following information.

* `next`. The corresponding URL is the link to the next page.
* `prev`. The corresponding URL is the link to the previous page.
* `last`. The corresponding URL is the link to the last page.

Note that when this header is not set, there is only one page, the first page, of results. You may specify how many records you want to recieve with the `per_page` parameter.

# Conversations

## The Conversation Object
```json
  {
    "id": 1,
    "external_id": "21212",
    "created": 1522874599,
    "tickets": [
          {
            "id": 1,
            "external_id": "564652",
            "conversation_id": 21212,
            "source": "zendesk",
            "original_text": "Hey Mary. \n Would it be possible to delete a user from
            the company settings page? \n Best, \n Robert \n CEO of Rosemary Inc. \n",
            "cleanest_body": "Would it be possible to delete a user from the company settings page?",
            "tag_relationships": [
              {
                "tag_id": 4,
                "salience": 0.7,
                "sentiment": 0.6
              },
              {
                "tag_id": 8,
                "salience": 0.2,
                "sentiment": 0.1
              }
            ]
          }
    ]

  }
```

A Conversations represents a collection of Tickets from the same user around the same time. The definition of a Conversation often depends on the system of record from which the Ticket comes from.

### Notable Attributes


Attribute | Description
--------- | ------- | -----------
source | The system of record from where the conversation was imported.
external_id | The id of the conversation from the system of record from where the conversation was imported.
tickets | The set of Tickets corresponding to that Conversation.

## Create a Conversation

```shell
curl "https://api.getscopeai.com/v1/conversation" -X POST
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command expects JSON structured like this:

```json
 {
    "external_id": "564652",
    "created": 1522874599
  }
```

This endpoint creates a Conversation.

### HTTP Request

`POST https://api.getscopeai.com/v1/conversation`


### JSON Body Parameters

Parameter   | Required |  Type       | Description
------------| ---------|  ---------- |-----------
external_id | false    | String    | ID of conversation for an external system of record. Should be a of type
created     | false    | Integer | Time at which the object was created. Measured in seconds since the Unix epoch.


## Get a Specific Conversation

```shell
curl "https://api.getscopeai.com/v1/conversation/3"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 3,
    "external_id": "21212",
    "created": 1522874599,
    "tickets": [
          {
            "id": 1,
            "external_id": 564652,
            "conversation_id": 21212,
            "source": "zendesk",
            "original_text": "Hey Mary. \n Would it be possible to delete a user from
            the company settings page? \n Best, \n Robert \n CEO of Rosemary Inc. \n",
            "cleanest_body": "Would it be possible to delete a user from the company settings page?",
            "tag_relationships": [
              {
                "tag_id": 4,
                "salience": 0.7,
                "sentiment": 0.6
              },
              {
                "tag_id": 8,
                "salience": 0.2,
                "sentiment": 0.1
              }
            ]
          }
    ]

  }

```

This endpoint retrieves a specific Conversation.

### HTTP Request

`GET https://api.getscopeai.com/v1/conversation/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Conversation to retrieve


## Find a Specific Conversation

```shell
curl "https://api.getscopeai.com/v1/conversations/find_by?external_id=L3123
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
 {
    "id": 3,
    "external_id": "L3123",
    "created": 1522874599,
    "tickets": [

    ]

  }
```

This endpoint retrieves a specific Conversation by the parameters specified in the request.

### HTTP Request

`GET https://api.getscopeai.com/v1/conversations/find_by`

### URL Parameters

Parameter | Description
--------- | -----------
external_id | The ID of the Conversation from the system of record from where the ticket was imported.


# Tickets

## The Ticket Object
```json
  {
    "id": 1,
    "external_id": "564652",
    "conversation_id": 21212,
    "created": 1522874599,
    "source": "zendesk",
    "original_text": "Hey Mary. \n Would it be possible to delete a user from
    the company settings page? \n Best, \n Robert \n CEO of Rosemary Inc. \n",
    "cleanest_body": "Would it be possible to delete a user from the company settings page?",
    "tag_relationships": [
      {
        "tag_id": 4,
        "salience": 0.7,
        "sentiment": 0.6
      },
      {
        "tag_id": 8,
        "salience": 0.2,
        "sentiment": 0.1
      }
    ]

  }
```

A ticket represents a message from a customer to a company. ScopeAI's definition of a ticket
is likely to be equivalent to your system of record's definition of a ticket.

### Notable Attributes


Attribute | Description
--------- | ------- | -----------
source | The system of record from where the customer service ticket was imported.
external_id | The id of the ticket from the system of record from where the ticket was imported.
conversation_id | An id to identify the relationship between tickets that are in the same conversation. For example all tickets from one chat session.
created | Time at which the object was created. Measured in seconds since the Unix epoch.
original_text | The text of the ticket that was received from the system of record.
cleanest_body | The ticket body after ScopeAI has scrapped off headers, footers, and other extraneous text.
tag_relationships | A list of objects that represent the relationship this ticket has with a tag. Contains the `tag_id` which can be used to obtain the tag object, the `salience_score` which reflects the tags relevance to the overall text and `sentiment_score` which, if available, represents the sentiment of said tag.



## Create a Ticket

```shell
curl "https://api.getscopeai.com/v1/tickets" -X POST
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command expects JSON structured like this:

```json
 {
    "external_id": "564652",
    "conversation_id": 32311,
    "sender": "robert@foo.com"
    "created": 1522874599,
    "original_text": "Hey Gary. \n What's the price of adding another superuser? \n Best, \n Bill \n CEO of Mirrors Inc. \n",
    "original_html": "<div> Hey Gary. <\br> What's the price of adding another superuser? <\br> Best, <\br> Bill \n CEO of Mirrors Inc. \n </div>",
  }
```

This endpoint creates a Ticket.


### HTTP Request

`POST https://api.getscopeai.com/v1/tickets`


### JSON Body Parameters

Parameter | Required | Type | Description
--------- | ------- |  ------- |-----------
external_id | false | String |  ID of ticket for an external system of record
conversation_id | true | Integer | The ID of the conversation this ticket belongs to.
sender | false | String | An identifier for the customer who sent the email. You may use email, username, or a numerical representation.
created | false | Integer | Time at which the object was created. Measured in seconds since the Unix epoch.
original_text | false | String | Ticket text.
original_html | false | String |HTML representation of ticket.


## Get All Tickets

```shell
curl "https://api.getscopeai.com/v1/tickets?start_date=1502607600"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "external_id": "564652",
    "conversation_id": 21212,
    "created": 1522874599,
    "source": "zendesk",
    "original_text": "Hey Mary. \n Would it be possible to delete a user from the company settings page? \n Best, \n Robert \n CEO of Rosemary Inc. \n",
    "cleanest_body": "Would it be possible to delete a user from the company settings page?",
    "tag_relationships": [
      {
        "tag_id": 4,
        "salience": 0.7,
        "sentiment": 0.6
      },
      {
        "tag_id": 8,
        "salience": 0.2,
        "sentiment": 0.1
      }
    ]
  },
  {
    "id": 3,
    "external_id": "564652",
    "conversation_id": 32311,
    "created": 1522874599,
    "source": "zendesk",
    "original_text": "Hey Gary. \n What's the price of adding another superuser? \n Best, \n Bill \n CEO of Mirrors Inc. \n",
    "cleanest_body": "What's the price of adding another superuser?",
    "tag_relationships": [
      {
        "tag_id": 32,
        "salience": 0.9,
        "sentiment": 0.3
      },
      {
        "tag_id": 11,
        "salience": 0.4,
        "sentiment": 0.6
      }
    ]

  }
```

This endpoint retrieves all Tickets.

### HTTP Request

`GET https://api.getscopeai.com/v1/tickets`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
start_date | 14 days ago | Return only tickets that were created after this timestamp. Timestamp must be in UNIX time.
end_date | Current timedate | Return only tickets that were created before this timestamp. Timestamp must be in UNIX time.




## Search Tickets

```shell
curl "https://api.getscopeai.com/v1/search?start_date=1502607600?query=price"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "external_id": "89652",
    "conversation_id": 22312,
    "source": "zendesk",
    "created": 1522874599,
    "original_text": "Hey John. \n Is it possible to reduce the price? \n Best, \n Francis \n CTO of EndUp Inc. \n",
    "cleanest_body": " Is it possible to reduce the price? ",
    "tag_relationships": [
      {
        "tag_id": 9,
        "salience": 0.4,
        "sentiment": 0.2
      },
      {
        "tag_id": 63,
        "salience": 0.9,
        "sentiment": 0.1
      }
    ]
  },
  {
    "id": 3,
    "external_id": "564652",
    "conversation_id": 32311,
    "source": "zendesk",
    "original_text": "Hey Gary. \n What's the price of adding another superuser? \n Best, \n Bill \n CEO of Mirrors Inc. \n",
    "cleanest_body": "What's the price of adding another superuser?",
    "tag_relationships": [
      {
        "tag_id": 32,
        "salience": 0.9,
        "sentiment": 0.3
      },
      {
        "tag_id": 11,
        "salience": 0.4,
        "sentiment": 0.6
      }
    ]

  }
```

This endpoint retrieves all tickets that match the given query. The query is searched against both the `original_text` and `cleanest_body`.

### HTTP Request

`GET https://api.getscopeai.com/v1/tickets/search`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
query | nil | The set of words that you want to search for in tickets
start_date | 14 days ago | Return only tickets that were created after this timestamp. Timestamp must be in UNIX time.
end_date | Current timedate | Return only tickets that were created before this timestamp. Timestamp must be in UNIX time.


## Get a Specific Ticket

```shell
curl "https://api.getscopeai.com/v1/tickets/3"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
 {
    "id": 3,
    "external_id": "564652",
    "conversation_id": 32311,
    "created": 1522874599,
    "source": "zendesk",
    "original_text": "Hey Gary. \n What's the price of adding another superuser? \n Best, \n Bill \n CEO of Mirrors Inc. \n",
    "cleanest_body": "What's the price of adding another superuser?",
    "tag_relationships": [

    ]

  }
```

This endpoint retrieves a specific Ticket.

### HTTP Request

`GET https://api.getscopeai.com/v1/tickets/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Ticket to retrieve


## Find a Specific Ticket

```shell
curl "https://api.getscopeai.com/v1/tickets/find_by?external_id=564652"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
 {
    "id": 3,
    "external_id": "564652",
    "conversation_id": 32311,
    "created": 1522874599,
    "source": "zendesk",
    "original_text": "Hey Gary. \n What's the price of adding another superuser? \n Best, \n Bill \n CEO of Mirrors Inc. \n",
    "cleanest_body": "What's the price of adding another superuser?",
    "tag_relationships": [

    ]

  }
```

This endpoint retrieves a specific ticket by the parameters specified in the request.

### HTTP Request

`GET https://api.getscopeai.com/v1/tickets/find_by`

### URL Parameters

Parameter | Description
--------- | -----------
external_id | The ID of the ticket from the system of record from where the ticket was imported.
conversation_id | An ID to identify the relationship between tickets that are in the same conversation. For example all tickets from one chat session.

## Add Tag to a Ticket

```shell
curl "https://api.getscopeai.com/v1/tickets/3/add_tag" -X POST
  -H "Authorization: Token token=pLac3keYh3r3"
```


> The above command expects JSON structured like this:

```json
  {
    "tag_id": 3,
  }
```

This endpoint adds a `Tag` to a Ticket


### HTTP Request

`POST https://api.getscopeai.com/v1/tickets/3/add_tag`

### JSON Body Parameters

Parameter | Required | Description
--------- | ------- | -----------
tag_id | true | The ID of the tag to be added to this ticket.

# Tags

## The Tag Object
```json
  {
    "id": 1,
    "name": "settings",
    "created": 1522874599,
    "score": 3,
    "tickets_count": 5,
    "relevant": true

  }
```

A tag represents an entity or 'subject' that has been discovered across multiple customer conversations.

### Notable Attributes


Attribute | Description
--------- | ------- | -----------
name | A unique textual representation of the tag.
score | A number representing how salient the tag is to a company.
tickets_count | The number of tickets that this tag is associated with.
relevant | A boolean representing whether the tag is relevant to the company.
similar_tags | A list of tags that are semantically similar to that tag.

## Create a Tag

```shell
curl "https://api.getscopeai.com/v1/tags" -X POST
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command expects JSON structured like this:

```json
  {
    "name": "settings",
    "relevant": true
  }
```

This endpoint creates a Tag.


### HTTP Request

`POST https://api.getscopeai.com/v1/tags`


### JSON Body Parameters

Parameter | Required | Type | Description
--------- | ------- |  ------- |-----------
name | true | String | A unique textual representation of the tag.
relevant | false | Boolean | A boolean representing whether the tag is relevant to the company. If not supplied it will default to `true`



## Get All Tags

```shell
curl "https://api.getscopeai.com/v1/tags?relevant=true"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "settings",
    "created": 1522874599,
    "score": 3,
    "tickets_count": 5,
    "relevant": true

  },
  {
    "id": 4,
    "name": "superuser",
    "created": 1522874599,
    "score": 4,
    "tickets_count": 12,
    "relevant": true
  }
]
```

This endpoint retrieves all Tags.

### HTTP Request

`GET https://api.getscopeai.com/v1/tags`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
relevant | true | Return only tickets that are relevant to a company.

## Get a Specific Tag

```shell
curl "https://api.getscopeai.com/v1/tags/4"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
 {
    "id": 4,
    "name": "superuser",
    "created": 1522874599,
    "score": 4,
    "tickets_count": 12,
    "relevant": true
  }
```

This endpoint retrieves a specific Tag.

### HTTP Request

`GET https://api.getscopeai.com/v1/tags/7`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the Tag to retrieve

## Find a Specific Tag

```shell
curl "https://api.getscopeai.com/v1/tags/find_by?name=house"
  -H "Authorization: Token token=pLac3keYh3r3"
```

> The above command returns JSON structured like this:

```json
 {
    "id": 343,
    "name": "house",
    "created": 1522874599,
    "score": 4,
    "tickets_count": 12,
    "relevant": true
  }
```

This endpoint retrieves a specific Tag using the parameters specified in the request.

### HTTP Request

`GET https://api.getscopeai.com/v1/tags/find_by`

### URL Parameters

Parameter | Description
--------- | -----------
name | The textual representation of the tag.
