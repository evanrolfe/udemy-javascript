# Section 2: Execution Contexts and Lexical Environments

- Syntax Parser
- Execution Context: which lexical environment are we in?
- Lexical Environment
- Object: simply a collection of key/value pairs

**Global Environment & Object**
- Global means "not inside a function"
- Global Context: global object `this`
- `window == this`
- All global vars and functions belong to `this`

**Creation and Hoisting**
```js
b();
console.log(a);
var a = 'Hello World!';

function b() {
  console.log("Called b()");
}
```

Results in:
```
> Called b()
> unedfined
```

- Variables and functions are hoisted / "moved to the top"
- Execution Context is created in two phases:
- (1) Creation Phase: setup memory space for variables and functions
  - functions placed in memory
  - variables declared but not assigned (set to undefined)
- (2) Code Execution Phase
  - line by line execution

**Undefined**
```js
var a;
console.log((a === undefined) ? 'Yes' : 'No);
// => Yes
```
- Undefined is a special value

**Function Invocation & Execution Stack**
- Stack: Each execution context is placed on top of each other starting with the global context
- Every function creates a new execution context, runs through Create phase then Execution phase
- When each function finishes, the execution context is popped off the stack

**Functions, Context and Environment Variables**
- Variable Environment: where the variables live & how they relate to each other in memory
```js
function b() {
  var myVar;
  // myVar == undefined
}

function a() {
  var myVar = 2;
  b();
  // myVar == 2
}

var myVar = 1;
// myVar == 1
a();
// myVar == 1
```

**Scope Chain**
```js
function b() {
  console.log(myVar);
  // => 1
}

function a() {
  var myVar = 2;
  b();
}

var myVar = 1;
a();
```
- Outer Environment Reference of b() context is the global context
- If JS doesn't find a var in current exec context, it will look in the outer context
- Lexical scope = where was the code physically written
- Outer reference depends on where the function sits lexically
- b() function sits lexically in the global context, not inside a()
- Change lexical scope of b():
```js
function a() {
  function b() {
    console.log(myVar);
    // => 2
  }

  var myVar = 2;
  b();
}

var myVar = 1;
a();
```

**Scope, ES6, Let**
- Scope: where a variable is available in the code
- let: only defines var inside a  block
- like var but won't let you call the var before the declaration line
- blocks: defined by curly braces (if, for, function etc.)

**Asynchronous Callbacks**
- Asynchronous: more than one at a time
- async callbacks are only processed once the existing exec stack is cleared
```js
// long running function
function waitThreeSeconds() {
    var ms = 3000 + new Date().getTime();
    while (new Date() < ms){}
    console.log('finished function');
}

function clickHandler() {
    console.log('click event!');
}

// listen for the click event
document.addEventListener('click', clickHandler);


waitThreeSeconds();
console.log('finished execution');
```
That results in:
```
> finished function
> finished execution
> click event!
```
