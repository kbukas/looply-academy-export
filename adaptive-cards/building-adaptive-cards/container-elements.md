---
description: >-
  Adaptive Card elements used to configure and organise an Adaptive Cards
  layout.
---

# Container Elements

## Container

#### Description

This is the standard container that can hold any and all other elements. This container is very useful if you have a single column of elements that you may need to render. Great for background images/colours, and alignment of child elements - all controllable from the [**Element Properties Toolbar**](./#element-properties-toolbar).&#x20;

#### Technical Documentation

[**Read more about the Container**](https://adaptivecards.io/explorer/Container.html)

## ImageSet

#### Description

Used for adding a collection of images. Each image takes its own column - acts as a carousel of images. You must use an HTTP URL for each of the images. You can add new images by selecting the ImageSet from the [**Adaptive Card Canvas**](./#adaptive-card-canvas) and clicking on the image icon on the bottom right of the selected element.

#### Technical Documentation

[**Read more about the ImageSet**](https://adaptivecards.io/explorer/ImageSet.html)

## FactSet

#### Description

Used as a 2-column table of information. Mimics a key-value table. Each new key-value pairing is a new row in the FactSet. This only allows text in both key and value text blocks. Add new fact sets from the [**Element Properties Toolbar**](./#element-properties-toolbar)**.**

#### Technical Documentation

[**Read more about the FactSet**](https://adaptivecards.io/explorer/FactSet.html)

## ColumnSet

#### Description

This container will allow you to add multiple columns. Gives the opportunity to add more elements or containers side by side. You can add a new column by clicking on the ColumnSet that has been added to the [**Adaptive Card Canvas**](./#adaptive-card-canvas) and selecting the icon at the bottom right. You can add any element to a column of a ColumnSet.

Each column can also have a weight set against it by changing the **Width** parameter in the [**Element Properties Toolbar**](./#element-properties-toolbar) to Weighted from Stretched and assigning a weight to the Weight field, this will be a percentage value.&#x20;

Columns share the same styling, and alignment features as the standard [**Container**](container-elements.md#container) element.

#### Technical Documentation

[**Read more about the ColumnSet**](https://adaptivecards.io/explorer/ColumnSet.html)

## Table

#### Description

Aligns elements in a table format.&#x20;

You can add rows by selecting the **Table** on the [**Adaptive Card Canvas**](./#adaptive-card-canvas) and then clicking the "**Add a row**" button. Remove a row by selecting the TableRow from the [**Card Structure Toolbar**](./#card-structure-toolbar) and then click either the "**X**" on the selected row or by pressing "**Backspace.**"

Adding and removing columns for a row is easy as well, select the **Table**, navigate to the [**Element Properties Toolbar**](./#element-properties-toolbar), scroll to the bottom, and add/remove columns.&#x20;

Each column can have its own weight - this is a proportional size against the other columns. e.g. Column A could be size 2 and Column B has a size of 1, this means Column A is 2x bigger than Column B.

Each TableCell, TableRow, and the Table itself can be styled like the base Container Element.

#### Technical Documentation

[**Read more about the Table**](https://adaptivecards.io/explorer/Table.html)
