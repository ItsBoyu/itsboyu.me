---
title: Learning Functional Programming with JS - Anjana Vakil
date: "2020-08-06T11:00:37.121Z"
template: "post"
draft: false
slug: "Learning Functional Programming with JS"
category: "程式學習"
tags:
  - "筆記練習"
description: ""
socialImage: ""
---


[![An introduction to functional programming](http://img.youtube.com/vi/e-5obm1G_FY/0.jpg)](https://www.youtube.com/watch?v=e-5obm1G_FY "An introduction to functional programming")

### What’s functional programming?
- a programming paradigm
- a coding style
- a mindset
- a buzz-wordy trend ( ?

### Why functional JavaScript?
- Object - Oriented JS gets tricky, such as prototypes, this…etc
- Safer, easier to maintain
- Established community built lot of libraries

## Let's get started!
### Do Everything with Functions  
> Take Input => Give Output  

Thinking about the flow of data through the program, instead of object and how they interact.

```javascript
  // Not functional, imperative style
    var name = "Boyu";
    var greeting = "Hi, I'm";
    console.log(greeting + name);  // => "Hi, I'm Boyu"

  // Functional
    function greeting(name) {
      return `Hi, I'm ${name}`;
    }

    greeting("Boyu"); // => "Hi, I'm Boyu"
```
### Avoid side effects - Use "pure" functions
> Let function to do nothing except take the input, compute the output and return it
- Print to the console is not returning the output
- Avoid to use some global scope variable to compute

```javascript
  // Not pure
    var name = "Boyu";
    function greet() {
      console.log("Hi, I'm" + name);
    }

  // Pure
    function greeting(name) {
      return `Hi, I'm ${name}`;
    }
```

### Use higher-order functions - Functions can be inputs / outputs
> Function can takes as inputs to other functions or be returned as outputs

```javascript
    function makeAdjectifier(adjective) {
      return function (string) {
        return adjective + ' ' + string
      };
    }

    var coolifier = makeAdjectifier('cool');
    coolifier('conference'); // => 'cool conference'
```

### Don’t iterate - Use map, reduce, filter
> Not only get the list, but also get the function that we’re going to apply to.

### Avoid mutability - Use immutable data
```javascript
  // Mutation (Bad!)
    var rooms = ['H1', 'H2', 'H3'];
    rooms[2] = 'H4';
    rooms; // => ['H1', 'H2', 'H4']

  // No mutation (Good!)
    var rooms = ['H1', 'H2', 'H3'];
    var newRooms = rooms.map(function(rm) {
      if (rm === 'H3') { return 'H4'; }
        else { return rm; }
      });
    newRooms; // => ['H1', 'H2', 'H4'] 
    rooms; // => ['H1', 'H2', 'H3']   
```

### Persistent data structures for efficient immutability
- To copy the whole data structure is not efficient
- Structural sharing: share parts of old versions with the new one
- The libraries recommended: [Mori](https://swannodette.github.io/mori/), [Immutable.js](https://immutable-js.github.io/immutable-js/), [Ramda](https://ramdajs.com/), [Lodash](https://lodash.com/), [Underscore](https://underscorejs.org/)

### 延伸閱讀
[An introduction to functional programming](https://codewords.recurse.com/issues/one/an-introduction-to-functional-programming)  

