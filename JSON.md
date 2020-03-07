# [JSON - JavaScript Object Notation](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON)

## JSON Features

1. light-weight
2. language independent
3. Text based (easy to read and write), human readable data exchange format

## JSON vs XML

```json
{"students":[
   {"name":"John", "age":"23", "city":"Agra"},
   {"name":"Steve", "age":"28", "city":"Delhi"},
   {"name":"Peter", "age":"32", "city":"Chennai"},
   {"name":"Chaitanya", "age":"28", "city":"Bangalore"}
]}
```

```xml
<students>
  <student>
    <name>John</name> <age>23</age> <city>Agra</city>
  </student>
  <student>
    <name>Steve</name> <age>28</age> <city>Delhi</city>
  </student>
  <student>
    <name>Peter</name> <age>32</age> <city>Chennai</city>
  </student>
  <student>
    <name>Chaitanya</name> <age>28</age> <city>Bangalore</city>
  </student>
</students>
```

## JSON data structure types

1) JSON objects

```json
var chaitanya = {
  "name" : "Chaitanya Singh",
  "age" : "28",
  "website" : "beginnersbook"
};
```

2) JSON objects in array

```json
var students = [{
   "name" : "Steve",
   "age" :  "29",
   "gender" : "male"

},
{
   "name" : "Peter",
   "age" : "32",
   "gender" : "male"

},
{
   "name" : "Sophie",
   "age" : "27",
   "gender" : "female"
}];
```

3) Nesting of JSON objects

```json
var students = {
    "steve" : {
        "name" : "Steve",
        "age" :  "29",
        "gender" : "male" 
    },

    "pete" : {
        "name" : "Peter",
        "age" : "32",
        "gender" : "male"
    },

    "sop" : {
        "name" : "Sophie",
        "age" : "27",
        "gender" : "female"
    }
}
```

## Syntax Rules

1. Data is in name/value pairs
2. Data is separated by commas
3. Curly braces hold objects
4. Square brackets hold arrays

## JSON Data - A Name and a Value

A name/value pair consists of a field name (in double quotes), followed by a colon, followed by a value:

```json
"name":"John"
```

**Note**: JSON names require double quotes. JavaScript names don't.

## JSON - Evaluates to JavaScript Objects

The JSON format is almost identical to JavaScript objects.

In JSON, keys must be strings, written with double quotes.

JSON
```json
{"name":"John"}
```

JavaScript
```javascript
{name:"John"}
```

## JSON Values

In JSON, values are must be one of the following data types:

1. a string
2. a number
3. an object (JSON object)
4. an array ([])
5. a boolean
6. null

```json
{"name":"John"} // string
{"age":30}      // number
{"employee":{"name":"John", "age": 30, "city":"New York"}}  // object
{"employee":["John", "Anna", "Peter"]}  // array
{"sale": true}  // boolean
{"middlename": null}    // null

In JavaScript, values can be all the above, plus any other valid JavaScript expression, including:

1. a function
2. a date
3. undefined

In JSON, string values must be written with double quotes.

In JavaScript, you can write string values with double or single quotes.

## JSON uses JavaScript Syntax

Because JSON syntax is derived from JavaScript object notation, very little extra software is needed to work with JSON within JavaScript.

With JavaScript you can create an object and assign data to it, like this:

```javascript
var person = { name: "John", age: 31, city: "New York" };
// return John
person.name;
// or
person[name];

person.name = "Gilbert";
person[name]= 'Gilbert';
```

## JSON Files

-The file type for JSON files is ".json"
-The MIME type for JSON text is "application/json"

## JavaScript Arrays as JSON



