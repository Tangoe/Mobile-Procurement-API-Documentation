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

## Contents of an Order Property XXXXXXXXXXXXXXXXXXXXXX

An individual order property contain the following required elements:

* **id** -- The unique ID identifing this specific property.





