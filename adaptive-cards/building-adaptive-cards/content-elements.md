---
description: >-
  Add content to your Adaptive Cards in the form of Text, Images, Videos and
  Buttons
---

# Content Elements

## TextBlock

#### Description

This is the standard TextBlock used to display basic text. This element has simple styling tools to change font weight, alignment, size, and colour as well as the maximum number of lines to display all controllable from the [**Element Properties** **Toolbar**](./#element-properties-toolbar).

#### Technical Documentation

[**Read more about the TextBlock**](https://adaptivecards.io/explorer/TextBlock.html)

## RichTextBlock

#### Description

A more advanced version of the TextBlock where you must use the [**Card Payload Editor**](./#card-payload-editor) to make changes and enhancements. The RichTextBlock will allow you to segment your content into sections allowing each section to be styled differently.

#### Technical Documentation

[**Read more about the RichTextBlock**](https://adaptivecards.io/explorer/RichTextBlock.html)

## Image

#### Description

Allows the ability to add an Image. Must be a valid HTTP URL pointing to an image. Can be styled from the [**Element Properties Toolbar**](./#element-properties-toolbar) and can have an actionable trigger if the image is clicked. [**See the Actions section for more.**](actions.md)

#### Technical Documentation

[**Read more about Image**](https://adaptivecards.io/explorer/Image.html)

## Media

#### Description

Attach video/audio files from external sites. You can attach multiple media options to the same **Media** element. Meaning you could have both an audio recording and a video within the same element.&#x20;

If there is a video linked to the Media element then you can add your own thumbnail to overlay over the video, if this is not supplied then the thumbnail from the video is used.

You can style the alignment of the Media element from the [**Element Properties Toolbar**](./#element-properties-toolbar).&#x20;

#### Technical Documentation

[**Read more about Media**](https://adaptivecards.io/explorer/Media.html)

## ActionSet

#### Description

A group of buttons aligned along a row. Each button will trigger an Action. [**Learn more about actions from the Action section**](actions.md). Adding buttons is easy: select the ActionSet and click **"Add an action"** and select from the dropdown, or go to the [**Element Properties Toolbar**](./#element-properties-toolbar) for the Action Set and select **"Add an action"** then select from the dropdown.&#x20;

Style the **ActionSet** from the [**Element Properties Toolbar**](./#element-properties-toolbar).

Each **Button** within the **ActionSet** can also be styled, simply click on the **Button** and alter it from the [**Element Properties Toolbar**](./#element-properties-toolbar).

#### Technical Documentation

[**Read more about an ActionSet**](https://adaptivecards.io/explorer/ActionSet.html)
