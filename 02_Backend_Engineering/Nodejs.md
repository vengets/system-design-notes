---
created: 2026-05-21 10:52
updated: 2026-05-21 10:52
tags:
  - backend
status: active
language:
source:
related:
url:
---


> [!info]
> Backend engineering concept or implementation pattern.

## Intro
 - Runs on V8 engine, V8 is developed using C++

## Key Concepts
- Functions are first class citizens - Treat functions as normal number, strings, .. and pass it to a function , assign it to a variable.

### Module.exports

#### Type 1
``` fn_file.js
var fn() {
	print('hi');
}
module.exports = fn;
```

``` consumer.js
var fn = require('fn_file');
fn()
```

#### Type 2
```
var greet2 = require('./greet2').greet;
greet2();
```

``` greet2.js
module.exports.greet = function() {
	console.log('Hello world!');
};
```

#### Type 3
```
var greet3 = require('./greet3');
greet3.greet();
greet3.greeting = 'Changed hello world!';
```

``` greet3.js
function Greetr() {
	this.greeting = 'Hello world!!';
	this.greet = function() {
		console.log(this.greeting);
	}
}

module.exports = new Greetr();
```

#### Type 4
```
var Greet4 = require('./greet4');
var grtr = new Greet4();
grtr.greet();
```

``` greet4.js
function Greetr() {
	this.greeting = 'Hello world!!!';
	this.greet = function() {
		console.log(this.greeting);
	}
}

module.exports = Greetr;
```

#### Type 5
```
var greet2 = require('./greet2').greet;
greet2();
```

``` greet2.js
module.exports.greet = function() {
	console.log('Hello world!');
};
```




### Prototype

```app.js
function Person(firstname, lastname) {
	
	this.firstname = firstname;
	this.lastname = lastname;
	
}

Person.prototype.greet = function() {
	console.log('Hello, ' + this.firstname + ' ' + this.lastname);
};

var john = new Person('John', 'Doe');
john.greet();

var jane = new Person('Jane', 'Doe');
jane.greet();

console.log(john.__proto__);
console.log(jane.__proto__);
console.log(john.__proto__ === jane.__proto__);
```