---
title: API Reference

language_tabs:
  - c#

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

Terms | Description
----- | -----------
Prime Condition | A PrimeCondition is one of possible situations of a monitored object, which means it happens or will happen on the monitored object. It will include a monitored object ID, a `logical operator`, and a `condition value`. 
NotificationCondtion | A NotificationCondition is a logical expression whose context contains prime conditions in it. It represents the condition—when should IASS notify receivers. If it returns TRUE, notification action will be triggered.
NotificationCondtion | A NotificationCondition is a logical expression whose context contains prime conditions in it. It represents the condition—when should IASS notify receivers. If it returns `TRUE`, notification action will be triggered.
NotificationAction | A Notification action is an action that will be triggered when notification conditions happen.
Logical operator | <ul><li>larger than ( > ): LARGET_THAN</li><li>smaller than ( < ): SMALLER_THAN</li><li>equal ( = ): EQUAL</li><li>not ( ! ): NOT</li><li>not less than ( >= ): NOT_LESS</li><li>not more than ( <= :) NOT_MORE</li></ul>
Condition value | It could be several forms: a word string, a boolean value, an integer, and a decimal. The type of condition values based on monitor object in interest.



# Request Functions

The functions described in this overview provide ways to set a monitor service request. From setting prime conditions, a possible condition of an object, for objects in interest to setting required notification condition, and then assign receivers accordingly.


### Constructor

> Use Example

> import functions for request IASS services, construct a request manager to call methods of setting and using monitor and notification services.

```c#
using IASS.Request

requestManager = New RequestFunctions( );
```

###Syntax

                    |
------------------- |
RequestFunctions( ) |

###Prarameters
This constructor does not have input parameters.

###Return Value
This constructor will not return value.



## createExpression

Creates an expression which composed by (monitor object ID, logical operator, condition value) and returns it in Expression data type.


> Use Example

> generate a logical expression for a monitor condition

```c#
priCon_expression = requestManager.createExpression(
  object12345,
  LARGER_THAN,
  30
);
```


###Syntax

                    |
------------------- |
Expression createLogicalExpression( `__In__  monitoredObjectID`, `__In__  logicalOperator`, `__In__  conditionValue` ); |


Prarameters | Description
----------- | -----------
monitoredObjectID [in]  | The ID of a monitor object. If the monitorObjectID matches the ID of an existing monitor object it will be accepted by the function; otherwise, OBJECT_EXIST_ERROR will be returned.
logicalOperator [in] | This parameter is one of elements in the operator set {`LARGER_THAN`, `SMALLER_THAN`, `EQUAL`, `NOT`, `NOT_LESS`, `NOT_MORE`} If the logical operator is not feasible for the type of condition value, LOGICAL_ERROR will be returned. Types and feasible logical operators: <ul><li>Integer (numerical):LARGER_THAN, SMALLER_THAN, EQUAL, NOT, NOT_LESS, NOT_MORE.</li><li>Decimal (numerical):LARGER_THAN, SMALLER_THAN, EQUAL, NOT, NOT_LESS, NOT_MORE.</li><li>Boolean value (not numerical):EQUAL, NOT.</li><li>String (not numerical):EQUAL, NOT.</li></ul>
conditionValue [in] | This parameter is a numerical value or a string which represent a Boolean result or a specific content in monitor object. If the type of condition value mismatch with the type of value in monitor object, TYPE_MISMATCH_ERROR will be returned.


###Return Value
If the function succeeds, the return value is an expression of a prime condition with information in input parameters. It will return through Expression data structure.

If the function fails, the return value is Null. 

###Exception

Exception | Description
--------- | -----------
TYPE_MISMATCH_EXCEPTION | The function will fail in several situations, one of them is type mismatching between the monitor object and the assigned condition value.
OUT_OF_RANGE_ EXCEPTION | The function will fail in several situations; one of them is that the assigned condition value is beyond possible range of the monitor object.
LOGICAL_ EXCEPTION | The function will fail when the logical operator is not feasible for the type of condition value.
OBJECT_EXIST_ EXCEPTION | The function will fail when the given monitor object ID does not in existing monitor objects, which represent the given monitor object may not exist.



## addPrimeCondition
To add one or more prime conditions that will be used to describe a notification condition for monitor and notification service ( in setNotificationCondtion).

> Use Example

> add new prime conditions

```c#
requestManager.addPrimeCondition(
  priCon_expression,
  priConManager. createExpression(object12345, EQUAL, 50),
  priConManager. createExpression(object12345, SMALLER_THAN, 100)
);

```


###Syntax

                    |
------------------- |
Void addPrimeCondition(`__In__  ExpressionArray`);|

Prarameters | Description
----------- | -----------
ExpressionArray [in] | It is an array of Expressions, each Expression represent a prime condition.

###Return Value
This constructor does not return a value.

###Exception
The function will fail when any of the input Expressions is not valid. According to different exceptions happen on Expressions, one of the following exceptions will report.

Exception | Description
--------- | -----------
TYPE_MISMATCH_EXCEPTION | If one of the input Expressions meets this situation, the function will fail: type mismatching between the monitor object and the assigned condition value.
OUT_OF_RANGE_ EXCEPTION | If one of the input Expressions meets this situation, the function will fail:  assigned condition value is beyond possible range of the monitor object.
LOGICAL_ EXCEPTION | If one of the input Expressions meets this situation, the function will fail: the logical operator is not feasible for the type of condition value.
OBJECT_EXIST_ EXCEPTION | If one of the input Expressions meets this situation, the function will fail: the given monitor object ID does not in existing monitor objects, which represent the given monitor object may not exist.


## getList
Retrieve the list of all added prime conditions.

> Use Example

```c#
//get a list of current monitored conditions
priCon_List = requestManager.getList( );

//check for empty
Bool isEmpty = ! priCon_List.Any( );
if (isEmpty){
  //empty
}
Else
{
  //if the list is not empty, it can be used through assign indexes
  print ( priCon_List[“object12345”][“LARGER_THAN”][“30”] );
}
```

###Syntax

                    |
------------------- |
PrimeConditionList getList( )|

###Prarameters
This function does not have input parameters.

###Return Value
If the function succeeds, the return value is a list of prime conditions. If the list is empty, which represents there is not previous added prime conditions, it will return Null.

The returned list is a List Array of prime conditions with indexes [monitorObjectID][logicalOperator][conditionValue] (*the definition of the three indexes, see the input parameter part in createExpression function). 

###Exception
No exception will happen in the function.


## getCondtionList
Retrieve the list of requested prime conditions for a monitored object.

> Use Example

```c#
//get a list of current monitored conditions for a monitor object
priCon_ListForObject = requestManager.getConditionList( “object12345”);

//check for empty
Bool isEmpty = ! priCon_ListForObject.Any( );
if (isEmpty){
  //empty
}
Else
{
  //if the list is not empty, it can be used through assign indexes
  print ( priCon_ListForObject [“LARGER_THAN”][“30”] );
}
```

Retrieve the list of requested prime conditions for a monitored object.

###Syntax

                    |
------------------- |
PrimeConditionList getConditionList( `__In__  monitorObjectID`);|

Prarameters | Description
----------- | -----------
monitorObjectID [in] | The ID of a monitor object. If the monitorObjectID matches the ID of an existing monitor object it will be accepted by the function; 

###Return Value
If the input monitorObjectID matches an existing monitor object’s ID, the function will succeed. The return value is a list of prime conditions, which related to the monitor object. If the list is empty, which represents there is no previous added prime conditions related to this monitor object, it will return Null.

The returned list is a List Array of prime conditions with indexes [logicalOperator][conditionValue] (*the definition of the two indexes, see the input parameter part in createExpression function). It can be used through assign indexes (see the use sample).

###Exception
This function will fail, when the input monitorObjectID does not exist. 

Exception   | Description
----------- | -----------
OBJECT_EXIST_ EXCEPTION | The function will fail when the given monitor object ID does not in existing monitor objects, which represent the given monitor object may not exist.



## setNotificationCondition
Sets notification conditions for request monitor and notification services. It is to specify that in what condition IASS should trigger notification actions.

> Use Example

```c#
//add a notification condition
noti_Con = requestManager.addNotificationCondition(
100,
(TODO)
);
```


###Syntax

                    |
------------------- |
NotificationConditionID addNewCondition(`__In__  responseTimeInterval` `__In  __  ContextString`);|

Prarameters | Description
----------- | -----------
responseTimeInterval [in] | It is the response time interval, in milliseconds. It represents the time tolerance between the happening of a notification condition and the receiving of notifications. If a nonzero value is specified, when a notification condition is detected happening, notifications should be received by receivers within the specified response time interval.
ContextString [in] | The ContextString is a description of a notification conditions, it follows the logical rules and tokens supported by Simple Rule Engine(1*).

(1*) Simple Rule Engine : SRE (Simple Rule Engine) is a lightweight forward chaining inference rule engine for .NET. Its 'simple' because of the simplicity in writing and understanding the rules written in XML, but this 'simple' engine can solve complex problems. 
http://sourceforge.net/projects/sdsre/
http://simpleruleengine.tripod.com/

(2*) Evaluation Engine: The Evaluation Engine is a parser and interpreter that can be used to build a Business Rules Engine. It allows for mathematical and boolean expressions, operand functions, variables, variable assignment, comments, and short-circuit evaluation. A syntax editor is also included.
http://www.codeproject.com/Articles/26314/Evaluation-Engine

(3*) Jess: Jess is a rule engine for the Java platform. To use it, you specify logic in the form of rules using one of two formats: the Jess rule language (prefered) or XML.
http://www.jessrules.com/doc/70/index.html

###Return Value
If the function succeeds, the return value will be the unique ID of the input notification condition.

If the function fails, the return value will be Null.

###Exception
If the function fails, according to different situations, it might have following exceptions.

Exception   | Description
----------- | -----------
TIME_INTERVAL_ EXCEPTION | The function will fail, if the required response time is lower than the lowest time interval that supported by IASS.
RULE_SYNTAX_EXCEPTION | The function will fail, if the input context expression not follows the supported syntax.
CONDITION_EXIST_EXCEPTION | The function will fail, if the one or more of conditions in the context expression cannot be found. That is, the assigned prime condition ID might be wrong.



## createNotificationAction
Create a notification action which composed by (notificationType, parameterArray) and returns it in NotificationAction data type. The Returned NotificationActionID is used to express an assigned notification action.

> Use Example

```c#
// to create a notification action
notiAct = requestManager.createNotificationAction (
WAKE_UP_PROCESS, 
processID98765
);
```

###Syntax

                    |
------------------- |
NotificationActionID createNotificationAction(`__In__  notificationType` `__In  __  parameterArray`);|

Prarameters | Description
----------- | -----------
notificationType [in] | It is the type of a notification action.
parameterArray [in] | It is an array of required parameters of a notification action. The numbers, types, and content of the parameters depends on the type of chosen notification action.


###Return Value
If the function succeeds, the return value will be the unique ID of the input notification action.

If the function fails, the return value will be Null.

###Exception
This function will fail, when the input notification parameters not match the chosen notification type.

Exception   | Description
----------- | -----------
PARAMETER_EXCEPTION | The exception will report, when the input parameters not match the chosen type of notification action.



###addNotificationAction
Set notification actions which should be taken when a specific notification condition happens.

> Use Example

```c#
//add notification actions. When the notification condition 11235 happens, do action001 and action002.
requestManager.addNotificationAction(
“notificationCondition11235”,
“action001”,
”action002”
);
```


###Syntax

                    |
------------------- |
Void assignPossibleSubscriber( `__In__  NotificationConditionID` , `__In__  NotificationActionList`);|


Prarameters | Description
----------- | -----------
NotificationConditionID [in] | It is the unique ID of a notification condition.
NotificationActionArray [in] | It is an array of notification actions.

###Return value
This constructor does not return a value.

###Exception
This function will fail, when the input notification condition or notification action do not exist.

Exception   | Description
----------- | -----------
CON_EXIST _EXCEPTION | If the input notification condition ID does not match any of existing notification conditions, the function will fail.
ACTION_EXIST_EXCEPTION | If one or more of input notification action IDs do not match any of existing notification actions, the function will fail.


# Browsing Functions



# Template_Kittens

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

