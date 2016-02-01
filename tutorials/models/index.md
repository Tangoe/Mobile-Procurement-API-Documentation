---
layout: default
title: Tutorial - Models
---


# Obtaining a List of Device Models

**This tutorial dscribes the steps for obtaining a list of models for a specific manufacturer.**

XXXXXXXXXX.
<br/>

## The /manufacturers/{id}/models endpoint

An API call can be made to the **/manufacturers/{id}/models** endpoint to obtain the known models that a particular manufacturer offers.


In order to obtain a list of known device models offered by a particular manufacturer, an API call must first be made to the **/manufacturers** endpoint to determine that manufacturer's ID. This ID is then passed into the models endpoint as a URI parameter identifying the selected manufacturer. This call will then return a response that includes all of the models known for this manufacturer. 


When making this API call, you can also set the (optional) **type** query parameter (e.g., ALL, DEVICE, ACCESSORY).


Here is an illustration of what the models might look like:

```

```

<br/>

## Contents of an Order Property

An individual order property contain the following required elements:

* **id** -- The unique ID identifing this specific property.

* **label** -- A descriptive label that is suitable for displaying to an end user.

* **validation** -- Indicates whether or not the property is required by the vendor.

* **type** -- Type indicates the type of data to which the value should be set:
  * *TEXT*: A string.
  * *DATE*: A date. (Note: the "pattern" element may be set to indicate the accepted format, e.g., ISO 8601, etc.)
  * *CHOICE*: An ID of a single option chosen from an array of choice options. 


They can also contain any of the following optional eleements:

* **maxLength** -- The maximum number of charaacters to which this property can be set. 

* **minLength** -- The minimum number of charaacters to which this property must be set. 

* **pattern** -- A description of the pattern to which the value should match (e.g, "mm/dd/yyyy"). This can be dislayed to an end user as a hint for what the properties value should look like.

* **regEx** -- A regular expression pattern that can be used to validate the value to which the proeprty is set.


<a name="#choiceProperty"></a><br/>

## The CHOICE Property Type

The CHOICE type is used to model properties that only accept a value from a defined list of options. An API consumer might use this property to surface a dropdown list in their user interface. 

CHOICE properties include an array named "choices" that contains its acceptable options. Each option in the array contains the following elements:

 * **id** -- The unique ID identifing this specific option.
 * **label** --  A descriptive label that is suitable for displaying to an end user.
 * **isOther** -- A boolean indicating if this specific option is used to select "other" (i.e., "none of the above"). There should only be one option in the array with isOther set to true. Furthermore, this element can be helpful to the user interface to trigger another event, such as revealing a text field for the end user to enter something not found in the dropdown list.

