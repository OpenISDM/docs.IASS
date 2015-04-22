---
title: API Reference

language_tabs:
  - c#

toc_footers:
  - <a href='#'>Authors: Rimi and Johnson Su</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - request
  - browsing

search: true
---

# Introduction

Welcome to the IASS API! You can use our API to access IASS API endpoints, which can browse the monitored objects, and also you can request monitored conditions for notification services to your application.

We have language bindings in C Sharp(C#)! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This documentaiton contains the reference for the IASS APIs and services offered by IASS for applications. These include available services for:

 - Request monitor and notification service
 - Browse monitor objects


<aside class="notice">This IASS API was designed as of April 2, 2015. Its last day of revision is April 14, 2015.</aside>


# Definitions

Terms | Description
----- | -----------
Prime Condition | A `Prime Condition` is one of possible situations of a monitored object, which means it happens or will happen on the monitored object. It will include a ID of `monitored object`, a `logical operator`, and a `condition value`. 
Notification Condtion | A `Notification Condition` is a logical expression whose context contains prime conditions in it. It represents the condition—when should IASS notify receivers. If it returns `TRUE`, notification action will be triggered.
Notification Action | A `Notification Action` is an action that will be triggered when notification conditions happen.
Logical operator | <ul><li>larger than ( > ): `LARGET_THAN`</li><li>smaller than ( < ): `SMALLER_THAN`</li><li>equal ( = ): `EQUAL`</li><li>not ( ! ): `NOT`</li><li>not less than ( >= ): `NOT_LESS`</li><li>not more than ( <= :) `NOT_MORE`</li></ul>
Condition Value | It could be several forms: a word string, a boolean value, an integer, and a decimal. The type of condition values based on monitor object in interest.



