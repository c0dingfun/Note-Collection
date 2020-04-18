# Javascript Notes

## [CSS](https://css-tricks.com/)

= Geneeral Roadmap

https://github.com/kamranahmedse/developer-roadmap

= Javascript

= Jonas is the best! (HTML, CSS, Javascript)
http://codingheroes.io/newsletter/
https://github.com/mbeaudru/modern-js-cheatsheet

https://blog.garstasio.com/you-dont-need-jquery/dom-manipulation/

http://keycodes.atjayjo.com/ (Keycodes)

https://medium.com/unicorn-supplies/angular-vs-react-vs-vue-a-2017-comparison-c5c52d620176

https://medium.com/ladies-storm-hackathons/how-we-built-our-first-full-stack-javascript-web-app-in-three-weeks-8a4668dbd67c


## Pluralsight

## Javascript: From Functional to Functional JS
   * slides: bit.ly/js102-slides1 (not working)
   * exercises: bit.ly/js102-exercises (not working)
   * exercises: https://github.com/bgando/JS102 (working)

### Objects && Property Access - {}

- Bracket notation

```javascript

   var box = {}
   box["material"] = "cardboard"; // assignment

   // equivalent to
   var box = { "material" : "cardboard" }

   box["material"]; // access - "cardboard"

   box["size"]; // undefined — a value of a type

   var key = "material";
   box[key]; // "cardboard"
```

- Dot notation


```javascript

   var box = {}
   box.material = "cardboard"; // assignment

   // equivalent to
   var box = { "material" : "cardboard" }

   box.material; // access - "cardboard"

   box.size; // undefined — a value of a type
```

- Dot vs Bracket
         
```javascript

   var box = {}
   box["material"] = "cardboard"; // assignment

   var key = "material";

   box['key'];  // undefined
   box.key;     // undefined
   box[key];    // cardboard

   box[0] = 'valid';
   box.0;   // invalid

   box['&&&'] = 'valid too';
   box.&&&; // invalid

   box = {
            'material' : 'cardboard',
            '0' : 'valid',
            '&&&' : 'valid too'
         };

```

```iecst

   > Dot notation
      > only string works (aka valid variable name)
      > No number
      > No quotation
      > No weird characters
      > No expressions

   > Bracket notation
      > " string, ok
      > number, ok
      > " weird character, ok
      > variables, ok
      > numbers, ok (no quote required)
      > expression, ok 

```
- Nested Objects
- Object Literals

```javascript

   box = 
   {
      'material' : 'cardboard',
      noQuoteNeeded : 'value', // no quote is ok

      '0' : 'valid', // require quote
      1 : 'valid', // no quote is ok

      '&&&' : 'valid too' // require quote
   };
      
```

- Iteration

```javascript

   for (var key in box){
      console.log(key); // material, noQuoteNeeded, 0, 1, &&&
   }

```

- Arrays - []
  
```iecst

   ◦ Arrays vs Objects
      ▸ Very similar, both are object
      ▸ Both can have numeric and string as property names
      ▸ Pretty much following the same rule - dot and bracket, for-in loop, as far as property is concerned.
      ▸ Note: regardless object or array, for-in always loop through object's property.
      ▸ Note: for loop —- for (var i = 0; i < x; i++) should be used to loop through an array's index
      ▸ Note: an object is not ordered, while an array does
      ◦ Access && Assignment
      ◦ Native Methods && Properties
         
            
      ◦ Iteration 
         ▸ Same as iteration an object's properties

   * Function - function(){}
      ◦ An array, arguments, available to function body

```

# Javascript Questions

   * Value vs Reference

# Javascript Tooling

   * jsbin.com
   * lodash.js (functional)
   * ramda.js (functional)
