---
title: "Write NextPageLink/Count for collection"
description: ""

---

From ODataLib 6.13.0, it supports to write the NextPageLink/Count instance annotation in top-level collection payload. Let's have an example:

When you want to serialize a collection instance, you should first create an object of `ODataCollectionStart`, in which you can set the next page link and the count value.

```C#

ODataMessageWriter messageWriter = new ODataMessageWriter(...);
IEdmTypeReference elementType = ...;
ODataCollectionWriter writer = messageWriter.CreateODataCollectionWriter(elementType);

ODataCollectionStart collectionStart = new ODataCollectionStart();

collectionStart.NextPageLink = new Uri("https://any");
collectionStart.Count = 5;

writer.WriteStart(collectionStart);

ODataCollectionValue collectionValue = ...;
if (collectionValue != null)
{
    foreach (object item in collectionValue.Items)
    {
        writer.WriteItem(item);
    }
}
writer.WriteEnd();
```

The payload looks like:

```C#
{
  "@odata.context":".../$metadata#Collection(...)",
  "@odata.count":5,
  "@odata.nextLink":"https://any",
  "value":[
    ...
  ]
}
```