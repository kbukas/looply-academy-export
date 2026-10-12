---
description: Adaptive Cards that act as Forms to retrieve data from an end user.
---

# Input Elements

## Input.Text

#### Description

A standard input field that will allow users to type in text. This input can be assorted with a Label and Placeholder.&#x20;

Regex validation is supported on this field to validate inputs. If validation fails the error message is then shown.&#x20;

This Input can have an **Inline Action** where the user can submit the data from this input field straight back to Looply for processing. [This can be any of the Actions from the Action section.](actions.md)

You can change this input to a multi-line input field where a larger response can be retrieved.&#x20;

Applying an **Id** to this input field will mean the data passed to Looply will be attributed to it. e.g An Id of `first_name` and an input field value of `John` will be passed to Looply as `{"first_name": "John"}.`

All of the above is edited using the [**Element Properties Toolbar**](./#element-properties-toolbar) that is found when clicking on the respective Input.Text field.

#### Technical Documentation

[**Read more about Input.Text**](https://adaptivecards.io/explorer/Input.Text.html)

## Input.Date

#### Description

A field designed for taking in dates from the end user.&#x20;

Provides a calendar for the end user to select a date.&#x20;

This input can be assorted with a Label and a Default value.&#x20;

You can supply a **Min** and a **Max** value for validation where if the user incorrectly adds a date will show the error message that is specified in the respective [**Element Properties Toolbar**](./#element-properties-toolbar)**.**&#x20;

All Dates added must follow the `YYYY-MM-DD` standard.

Applying an **Id** to this input field will mean the data passed to Looply will be attributed to it. e.g An Id of `date` and an input field value of `2023-10-17` will be passed to Looply as `{"date": "2023-10-17"}.`

All of the above is edited using the [**Element Properties Toolbar**](./#element-properties-toolbar) toolbar that is found when clicking on the respective Input.Date field.

#### Technical Documentation

[**Read more about Input.Date**](https://adaptivecards.io/explorer/Input.Date.html)

## Input.Time

#### Description

A field designed for taking in times from the end user. &#x20;

This input can be assorted with a Label and a Default value.&#x20;

You can supply a **Min** and a **Max** value for validation where if the user incorrectly adds a time will show the error message that is specified in the respective [**Element Properties Toolbar**](./#element-properties-toolbar).&#x20;

All Times added must follow the `MM:HH` standard.

Applying an **Id** to this input field will mean the data passed to Looply will be attributed to it. e.g An Id of `time` and an input field value of `15:30` will be passed to Looply as `{"time": "15:30"}.`

All of the above is edited using the [**Element Properties Toolbar**](./#element-properties-toolbar) that is found when clicking on the respective Input.Time field.

#### Technical Documentation

[**Read more about Input.Time**](https://adaptivecards.io/explorer/Input.Time.html)

## Input.Number

#### Description

A field designed for taking in numbers from the end user. &#x20;

This input can be assorted with a Label, placeholder, and a Default value.&#x20;

You can supply a **Min** and a **Max** value for validation where if the user incorrectly adds a number will show the error message that is specified in the respective [**Element Properties Toolbar**](./#element-properties-toolbar).&#x20;

Applying an **Id** to this input field will mean the data passed to Looply will be attributed to it. e.g An Id of `price` and an input field value of `1.10` will be passed to Looply as `{"price": "1.10"}.`

All of the above is edited using the [**Element Properties Toolbar**](./#element-properties-toolbar) that is found when clicking on the respective Input.Number field.

#### Technical Documentation

[**Read more about Input.Number**](https://adaptivecards.io/explorer/Input.Number.html)

## Input.ChoiceSet

#### Description

A field designed to replicate a dropdown menu.&#x20;

This input can be assorted with a Label, placeholder, and a Default value.&#x20;

If a user incorrectly selects or types a value not within the ChoiceSet scope of values then an error is shown. That error message is customised via the [**Element Properties Toolbar**](./#element-properties-toolbar).

There are a few different types of ChoiceSet: **Compact**, **Expanded**, and **Filtered.** This can be changed from the [**Element Properties Toolbar**](./#element-properties-toolbar) using the Style property.

* Compact
  * Follows the same style as a regular dropdown menu.
* Expanded
  * Changes the **ChoiceSet** into **radio buttons** or **checkboxes** depending on if **multi-selection** is selected.
* Filtered
  * Changes the **ChoiceSet** to appear as an Input.Text field but will autocomplete values if the user starts to type a value from the **ChoiceSet** array.

Applying the Multi-Selection option to the **ChoiceSet** will always change the **ChoiceSet** into a group of checkboxes where each checkbox points to a choice.

Applying an **Id** to this input field will mean the data passed to Looply will be attributed to it. e.g An Id of `choice` and an input field value of `1` will be passed to Looply as `{"choice": "1"}.`

All of the above is edited using the [**Element Properties Toolbar**](./#element-properties-toolbar) that is found when clicking on the respective Input.ChoiceSet field.

#### Technical Documentation

[**Read more about Input.ChoiceSet**](https://adaptivecards.io/explorer/Input.ChoiceSet.html)

## Input.Toggle

#### Description

Input to replicate a checkbox.&#x20;

This input can be assorted with a Label, a Title, and a Default value

Labels will go above the checkbox whilst Titles will go to the side.&#x20;

Specific values can be assigned when the Toggle has been toggled "On" or "Off"

Applying an **Id** to this input field will mean the data passed to Looply will be attributed to it. e.g An Id of `toggle` and an input field value of `true` will be passed to Looply as `{"toggle": "true"}.`

All of the above is edited using the [**Element Properties Toolbar**](./#element-properties-toolbar) that is found when clicking on the respective Input.Toggle field.

#### Technical Documentation

[**Read more about Input.Toggle**](https://adaptivecards.io/explorer/Input.Toggle.html)
