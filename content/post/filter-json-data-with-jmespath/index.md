---
title: Processing JSON data with JMESPath Part 2 - Filtering Projections

# Date published
date: "2022-04-23T00:00:00Z"


# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: true

# Featured image
# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ""
  placement: 2
  preview_only: false
  
  
authors:
- admin

tags:
- JMESPath
- DTO
- Spring DTO

categories: 
---

[Part 1](https://www.njari.dev/post/process-json-data-with-jmespath/) of this JMESPath series deals with basic expressions and creating list and slice projections.

In this part, we'll look at filtering the results of a projection.

As a guide, we will use example if else expressions/pseudocode to generate JMESPath expressions.

## Example Data
```json
{
    "data":[
	{   "id": "0001",
		"type": "donut",
		"name": "Cake",
		"ppu": 0.55,
        "colours":{
            "top":"brown",
            "bottom":"white"
        },
		"batters":
			{"batter":[ { "id": "1001", "type": "Regular" },
						{ "id": "1002", "type": "Chocolate" },
						{ "id": "1003", "type": "Blueberry" },
						{ "id": "1004", "type": "Devil's Food" }]
			},
		"topping":[ {"id": "5001", "type": "None" },
                    { "id": "5002", "type": "Glazed" },
                    { "id": "5005", "type": "Sugar" },
                    { "id": "5007", "type": "Powdered Sugar" },
                    { "id": "5006", "type": "Chocolate with Sprinkles" },
                    { "id": "5003", "type": "Chocolate" },
                    { "id": "5004", "type": "Maple" }]
	},
    {   "id": "0002",
		"type": "donut",
		"name": "Raised",
		"ppu": 0.55,
        "colours":{
            "top":"brown",
            "bottom":"white"
        },
		"batters":
			{"batter":[ { "id": "1001", "type": "Regular" },
						{ "id": "1002", "type": "Chocolate" },
						{ "id": "1003", "type": "Blueberry" },
						{ "id": "1004", "type": "Devil's Food" }]
			},
		"topping":[ {"id": "5001", "type": "None" },
                    { "id": "5002", "type": "Glazed" },
                    { "id": "5005", "type": "Sugar" },
                    { "id": "5007", "type": "Powdered Sugar" },
                    { "id": "5006", "type": "Chocolate with Sprinkles" },
                    { "id": "5003", "type": "Chocolate" },
                    { "id": "5004", "type": "Maple" }]
	},
	{   "id": "0003",
		"type": "donut",
		"name": "Raised",
		"ppu": 0.55,
        "colours":{
            "top":"brown",
            "bottom":"white"
        },
		"batters":
			{
                "batter":[{ "id": "1001", "type": "Regular" }]
			},
		"topping":[ { "id": "5001", "type": "None" },
                    { "id": "5002", "type": "Glazed" },
                    { "id": "5005", "type": "Sugar" },
                    { "id": "5003", "type": "Chocolate" },
                    { "id": "5004", "type": "Maple" }]},
	{   "id": "0004",
		"type": "donut",
		"name": "Old Fashioned",
		"ppu": 0.55,
        "colours":{
            "top":"brown",
            "bottom":"white"
        },
		"batters":
			{"batter":[ { "id": "1001", "type": "Regular" },
						{ "id": "1002", "type": "Chocolate" }]},
		"topping":
			[   { "id": "5001", "type": "None" },
				{ "id": "5002", "type": "Glazed" },
				{ "id": "5003", "type": "Chocolate" },
				{ "id": "5004", "type": "Maple" }]}
]
}
```

This object has the following structure, with four objects(donuts) with ids `0001` to`0003`:
```
data -> id
        type
        name
        ppu
        coulours -> top
                    bottom
        batters ->  id
                    type
        topping ->  id
                    type
```
### Scenario 1: Testing Equality - integers
We can get an element with id `0001` as in the pseudocode below:
```python
# assuming the json object is initialized as 'obj'
result = [i in obj where i.id = "0001"
```

the expression will be ```data[?id==`0001`]```

> **NOTE:** The backticks must be used on integers and the escape type might be dependent on the target OS. See [this](https://jmespath.org/specification.html#literal-expressions)
> for more details.


### Scenario 2: Testing equality - strings
We can get an element with name `Cake` as in the pseudocode below:
```python
# assuming the json object is initialized as 'obj'
result = [i in obj where i.name = "Cake"
```

the expression will be `data[?name=='Cake']`

### Scenario 3: Two filter conditions
We can get an element with name `Raised` (there are two) and id `0003` as in the pseudocode below:
```python
# assuming the json object is initialized as 'obj'
result = [i in obj where i.name = "Raised" && i.id = "0003"
```

the expression will be ```data[?name=='Raised'&& id ==`0003`]```

### Scenario 4: Getting only select fields.
Following the YAGNI principle, you may not need all fields in the result of your JMESPath queries.
For example, in our case we may only need the id field from the results of this filtered projection: `data[?name=='Raised']`.
In such case, we just add `.id` to the exprssion, like `data[?name=='Raised'].id` which gives us 
```json
[
  "0002",
  "0003"
]
```
To add more fields other than `id`, we use `data[?name=='Raised'].[id,type]` which gives us a list of lists as shown below:
```json
[
  [
    "0002",
    "donut"
  ],
  [
    "0003",
    "donut"
  ]
]
```
### Scenario 4: Rename fields
As part of a mapping resulting data to a DTO, we may require data to adhere to a certain naming standard. 
To change the name of fields, the expression can be modified as follows:

`data[?name=='Raised'].{pastry_id:id,type_of:type}` to give:
```json
[
  {
    "pastry_id": "0002",
    "type_of": "donut"
  },
  {
    "pastry_id": "0003",
    "type_of": "donut"
  }
]
```
### Scenario 5: Multiselect with some nested
To select some nested values, we can do: `data[?name=='Raised'].{pastry_id:id,type_of:type,top_colour:colours.top}` to get:
```json
[
  {
    "pastry_id": "0002",
    "type_of": "donut",
    "top_colour": "brown"
  },
  {
    "pastry_id": "0003",
    "type_of": "donut",
    "top_colour": "brown"
  }
]
```
### Scenario 5: Slicing results from projections of nested projections
We can obtain data from list projections further down the ree structure as shown: `data[?name=='Raised'].{pastry_id:id,type_of:type,top_colour:colours.top,first_topping_id:topping[0].id}`
which gives us the id of the first topping (in addition to the id, type, and top_colour) - all renamed - of items with `name` = `Raised`. as shown below:
```json
[
  {
    "pastry_id": "0002",
    "type_of": "donut",
    "top_colour": "brown",
    "first_topping_id": "5001"
  },
  {
    "pastry_id": "0003",
    "type_of": "donut",
    "top_colour": "brown",
    "first_topping_id": "5001"
  }
]
```



## Conclusion

This section covers the basics of filtering JSON data using JMESPath. 
But what happens if you do not want to just operate on the results of a projection to ach item in the list? Pipe expressions come in handy.

For example, `data[*].id` will give you a list of pastry ids, a list projection. What if you want to get the first item of this list? Try `data[*].id[0]`.
>**Hint:** The result will be `null`.
To get the first element, we use a pipe expression like `data[*].id | [0]`, which follows the pattern  `<expression> | <expression>`.

Part 3 goes into detail about pipe expressions.
