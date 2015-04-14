---
title: API Reference

language_tabs:
  - C#

toc_footers:
  - <a href='#'>Authors: Rimi and Johnson Su</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

Welcome to the IASS API! You can use our API to access IASS API endpoints, which can browse the monitored objects, and also you can request monitored conditions for notification services to your application.

We have language bindings in C Sharp(C#)! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


<aside class="notice">to be describe a box environment (POS)</aside>


# Definitions

Prime Conditions:| 
-----------------|
Prime conditions, which are possible situation of monitored object. A PrimeCondition is one of possible situations of a monitored object, which means it happens or will happen on the monitored object. It will include a monitored object ID, a logical operator parameter (LARGER_THEN, SMALLER_THAN, EQUAL, NOT, etc.), and a condition value. |

Notification Conditions:|
------------------------|
Notification conditions, which are when should IASS taken corresponding notification actions. A NotificationCondition is a logical expression which contains prime conditions in it. It represents the condition—when should IASS notify receivers. If it returns TRUE, notification action will be taken. |

Notification Actions:| 
---------------------|
Notification actions, whom will receive notifications and how will them receive notifications. A Notification is an action that will be taken when notification conditions happen.|

# Request Functions

The functions described in this overview provide ways to set a monitor service request. From setting prime conditions, a possible condition of an object, for objects in interest to setting required notification condition, and then assign receivers accordingly.

## createLogicalExpression

```C#
Use Example
```

Create an expression to express a prime condition used in addNewCondtion.


###Syntax

###Prarameters

###Return Value

###Requirements



## addPrimeCondition

```C#
Use Example
```

To add one or more prime conditions that will be used in setNotificationCondtion.


###Syntax

###Prarameters

###Return Value

###Requirements



## getList

```C#
Use Example
```

Retrieve the list of all requested prime conditions.


###Syntax

###Prarameters

###Return Value

###Requirements



## getCondtionList

```C#
Use Example
```

Retrieve the list of requested prime conditions for a monitored object.

###Syntax

###Prarameters

###Return Value

###Requirements


## getCondtion

```C#
Use Example
```

Retrieve a prime condition.

###Syntax

###Prarameters

###Return Value

###Requirements


## setNotificationCondition

```C#
Use Example
```

Sets notification conditions for request monitor and notification services. It is to specify that in what condition IASS should notify receivers.

###Syntax

###Prarameters

###Return Value

###Requirements


## addNotificationAction

```C#
Use Example
```

Set notification actions which should be taken when a specific notification condition happens.

###Syntax

###Prarameters

###Return Value

###Requirements





# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

