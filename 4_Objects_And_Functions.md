# Section 4: Objects and Functions

**Object Literals**
`var person = {};`
Same as:
`var person = new Object();`

**Functions are objects**
- "First class functions" - everything you can do with other types, you can do with functions:
  - assign them to variables, pass them around, create them on the fly
- Function name can be anonymous (no name)
- Functions are objects, who's code is one of the properties of that object and can be invoked
```js
function greet() {
  console.log('hello world');
}

greet.language = 'english';
```

**Function Statements & Expressions**
- Expression returns a value
- statement does not return a value
- anon function:
```js
var anonymousGreet = function() {
  console.log('Hello world');
}
```
- Pass functions as arguments:
```js
function log(a) {
  a();
}

log(function() { console.log('Hello!'); });
```

**Pass by Value vs Reference**
```js
b = a;
b(a);
```
- Primitives are cloned (by value)
- Objects are referenced

**Objects, Functions & This**
- When a function is called from global, `this` points to global execution context:
```js
function a() {
  console.log(this);
  this.aVar = 'hello';
}

a();
console.log(aVar);
// => hello
```
- this refers to object when inside a method:
```js
var c = {
  name: 'The c object',
  log: function() {
    this.name = 'Updated C object';
    console.log(this);
  }
};

c.log();
// => Object { name: 'Updated C object', log: function }
```
- weird quirk, when inside function inside a function, this points to global object
```js
var c = {
  name: 'The c object',
  log: function() {
    this.name = 'Updated C object';
    console.log(this);

    var setName = function(newName) {
      this.name = newName;
    };

    setName('Updated c object name again!');
    console.log(this);
  }
};

c.log();
console.log(this.name);

// => Object {name: "Updated C object", log: ƒ}
// => Object {name: "Updated C object", log: ƒ}
// => Updated c object name again!
```
- You can get around this by defining this in the log function:
```js
log: function() {
  var self = this;
```

**Immediately Invoked Function Expressions (IIFEs)**
```js
var greeting = function(name) {
  return 'Hello ' + name;
}('John');

console.log(greeting);
```
- This has its own execution context (not global)
- You could pass in global context if you wanted to:
```js
(function(global, name){
  console.log('Hello ' + name);
  global.greeting = 'hello';
}(window, 'John'));
```

**Closures**
"closing in outer variables"
```js
function greet(whatToSay) {
  return function(name) {
    console.log(whatToSay + name);
  }
}
var sayHi = greet('Hello ');
sayHi('Evan');
```
- The three functions all have the same lexical context, so all point to the same `i`:
```js
function buildFunctions() {
  var arr = [];

  for(var i = 0; i < 3; i++) {
    arr.push(function(){
      console.log(i);
    });
  }

  return arr;
}

var fs = buildFunctions();
fs[0]();
fs[1]();
fs[2]();

// Returns:
// > 3
// > 3
// > 3
```
- Use `let` instead of `var` to get block scope which would achieve this:
```
// > 0
// > 1
// > 2
```

**Call, Apply, Bind**
- All functions have these methods defined on them
- Bind returns a new function with `this` set to the argument of bind():
```js
var person = {
  firstName: 'Evan',
  lastName: 'Rolfe,
  getFullName: function() {
    return this.firstName + ' ' + this.lastName;
  }
};

var logName = function(lang1, lang2) {
  console.log('Logged: ' + this.getFullName());
};

var logPersonName = logName.bind(person);
logPersonName();
// => Logged: Evan Rolfe
```
- Call and apply same as bind but executes it and lets you pass parameters:
```js
logName.call(person, 'en', 'es')
logName.bind(person, ['en', 'es'])
```
- Function currying: creating a copy of a function but with some pre-set params
```js
function multiply(a, b) {
  return a * b;
}
var multiplyByTwo = multiply.bind(this, 2)
console.log(multiplyByTwo(4));
// => 8
```
