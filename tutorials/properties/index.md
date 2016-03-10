---
layout: default
title: Obtaining Order Properties
---


# Obtaining Order Properties

**This tutorial dscribes the steps for obtaining the required (and optional) properties that are supported by the vendor who will be fulfilling your order.**

<br/>

Order properties are additional pieces of information about your order. They are not used by the API to process the order. But rather, they are passed on to the third-party vendor who will be fulfilling your order. Some of these properties may be required to be populated in order for your order to be accepted.

<br/>

## The /orders/properties endpoint

An API call can be made to the **/orders/properties** endpoint to obtain the properties. The specific properties returned are determined based on the order transaction type and  vendor who will be fulfilling the order.

When making this API call, you must always set the **transactionType** query parameter (e.g., NEW_ACTIVATION, UPGRADE, SUSPEND, etc.). You also must also identify the vendor by passing either one of the two following query parameters:
 
* **vendor** -- The ID of the vendor who will be fulfilling the order. This ID can be obtained from mutliple other endpoints, including /vendors, /catalog/plans, etc.

* **service** -- The ID of a service asset for which the order is being executed, when applicable. The backend system will look up the ID of the vendor who is associated with this asset.


Here is an illustration of what the order properties response could look like:

```
{
  "properties": {
    "_meta": {
      "href": "https://tg-mobility.cloudhub.io/mobility/v1/orders/properties?transactionType=DEACTIVATE&service=123546789"
    },
    "items": [
      {
        "fields": [
          {
            "id": "CONTACT_NUMBER",
            "label": "Contact Phone Number",
            "maxLength": 20,
            "minLength": 0,
            "text": "2038599360",
            "type": "TEXT",
            "validation": "REQUIRED"
          },
          {
            "id": "ADDITIONAL_INSTRUCTIONS",
            "label": "Additional Instructions",
            "maxLength": 4000,
            "minLength": 0,
            "type": "TEXT",
            "validation": "OPTIONAL"
          },
          {
            "id": "PREFERRED_DEACTIVATION_DATE",
            "label": "Preferred Deactivation Date",
            "maxLength": 10,
            "minLength": 10,
            "pattern": "mm/dd/yyyy",
            "regEx": "(0[1-9]|1[012])[- /.](0[1-9]|[12][0-9]|3[01])[- /.]\\d\\d\\d\\d",
            "type": "DATE",
            "validation": "OPTIONAL"
          },
          {
            "choices": [
              {
                "id": "1",
                "isOther": false,
                "label": "Cancel service as employee is no longer with the company"
              }
            ],
            "id": "Reason",
            "label": "Reason",
            "type": "CHOICE",
            "validation": "REQUIRED"
          }
        ],
        "groupId": "MISCELLANEOUS"
      }
    ]
  }
}
```

<br/>

## Contents of an Order Property

An individual order property contains the following required elements:

* **id** -- The unique ID identifing this specific property.

* **label** -- A descriptive label that is suitable for displaying to an end user.

* **validation** -- Indicates whether or not the property is required by the vendor.

* **type** -- Indicates the type of data to which the value should be set:
  * *TEXT*: A string.
  * *DATE*: A date. (Note: the "pattern" element may be set to indicate the accepted format, e.g., ISO 8601, etc.)
  * *CHOICE*: An ID of a single option chosen from an array of choice options. 


They can also contain any of the following optional elements:

* **maxLength** -- The maximum number of characters to which this property can be set. 

* **minLength** -- The minimum number of characters to which this property must be set. 

* **pattern** -- A description of the pattern to which the value should match (e.g, "mm/dd/yyyy"). This can be displayed to an end user as a hint for what the properties value should look like.

* **regEx** -- A regular expression pattern that can be used to validate the value to which the property is set.


<a name="#choiceProperty"></a><br/>

## The CHOICE Property Type

The CHOICE type is used to model properties that only accept a value from a defined list of options. An API consumer might use this property to surface a dropdown list in their user interface. 

CHOICE properties include an array named "choices" that contains its acceptable options. Each option in the array contains the following elements:

 * **id** -- The unique ID identifing this specific option.
 * **label** --  A descriptive label that is suitable for displaying to an end user.
 * **isOther** -- A boolean indicating if this specific option is used to select "other" (i.e., "none of the above"). There should only be one option in the array with isOther set to true. Furthermore, this element can be helpful to the user interface for triggering another event, such as revealing a text field for the end user to enter something not found in the dropdown list.

