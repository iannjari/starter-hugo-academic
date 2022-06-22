---
title: Processing JSON data with JMESPath Part 1 - Basic Expressions and Projections

# Date published
date: "2022-04-21T00:00:00Z"


# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: true

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ""
  placement: 2
  preview_only: true

authors:
- admin

tags:
- JMESPath
- DTO
- Spring DTO

categories: 
---

[JMESPath](https://jmespath.org/) has made the processing of complex JSON objects a tad bit easier. A JSON document is deconstructed using 
simple yet very powerful expressions. The witers of JMESPath claim it is an ABNF grammar with a complete specification.

To follow this illustration, you can play around with these expressions on the playground provided on the library's home page.

## Basic Expressions
#### Accessing Nested Objects (Sub-expressions)


Using this object;

```json
{
    "level_one_key1":{
        "level_two_key1":"level_two_value1",
        "level_two_key2":"level_two_value2"
    },
    "level_one_key2":{

    }
}
```
to access values in the first nesting level, we just need to call the desired data's key as follows
```bash
level_one_key1
```
which gives us 
```json
{
  "level_two_key1": "level_two_value1",
  "level_two_key2": "level_two_value2"
}
```
For values in the second nesting, we just call them using the combine level one and two keys as follows;
```bash
level_one_key1.level_two_key1
```
and get:
```json
"level_two_value1"
```

If you use a key that does not exist, the result will be:
```json
null
```
#### Accessing Objects using Indices

To access the members of an array, indexing can be used. For example, to get `"b"` from:
```json
["a","b",{"a":"b"},"4"]
```
,
use:
```bash
[1]
```
and:
```bash
[2]
``` 
and so on.

#### Slicing Array Objects using Indices
Using the array from previous example, 
```
[1:3]
```
gives
```json
[
  "b",
  {
    "a": "b"
  }
]
```

## Wildcard Expressions - List and Slice Projections

A wildcard expression projects the values of a JSON object belonging to a value to a list array.
For example;
Given:
```json
{
  "items": [
    {"id": "3", "colour": "blue"},
    {"id": "4", "colour": "purple"},
    {"id": "5", "colour": "peach"},
    {"colour": "green"}
  ],
  "towns": {"name": "Thika"}
}
```
we can project all members of `"items"` to a list array as follows;
```bash
items[*]
```
to get:
```json
[
  {
    "id": "3",
    "colour": "blue"
  },
  {
    "id": "4",
    "colour": "purple"
  },
  {
    "id": "5",
    "colour": "peach"
  },
  {
    "colour": "green"
  }
]
```
Note that if `"items"` was nested as a value, like:
```json
{
    "data":{
  "items": [
    {"id": "3", "colour": "blue"},
    {"id": "4", "colour": "purple"},
    {"id": "5", "colour": "peach"},
    {"colour": "green"}
  ],
  "towns": {"name": "Thika"}
},
    "date":"values"
}
```
then the same projection as above should be achieved as follows;
```bash
data.items[*]
```
We can limit the resultant list to just IDs as follows `data.items[*].id` or colours like `data.items[*].colour`.

Note what happens to the last record of `"items"` when `.id` is replaced with `.colour`.

The list can be sliced as desired like `data.items[0:2].colour`. Note that the slicing happens first.

## Conclusion

This section covers the basics and projections of JSON data using JMESPath. 

[Part 2](https://www.njari.dev/post/filter-json-data-with-jmespath/) goes into detail about filtering.
