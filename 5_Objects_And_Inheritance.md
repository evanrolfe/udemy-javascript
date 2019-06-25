# Section 5: Objects and Inheritance

**Prototype**
- All objects have a reference to the `__proto__` property
- if a function call `this` in the parent object, then it will reference the child object

**Reflection & Extend**
- And object can access its properties
- `object.hasOwnProperty('prop')` checks that `prop` is defined on the object itself and not it's prototype
- `extend` adds properties to the object

**Function Constructors**
- New keyword:
```js
function Person(firstName, lastName) {
  this.firstName firstName;
  this.lastName = lastName;
}

Person.prototype.getFullName = function() {
  return this.firstName + ' ' + this.lastName;
}

var evan = new Person('Evan', 'Rolfe');
```
- constructors functions have prototype (only used by `new`) which is the prototype of any objects created by the constructor
- the prototype of `evan` is `Person.prototype`
- prefer to add functions to objects using Object.prototype because of memory space
- These are not equal!
```js
var a = 3;
var b = new Number(3);
```
- Better to use literals over constructors for primitives

**Object.create**
- Protypal inheritance: you make objects and then create new objects pointing to the existing ones and override properties
- `Object.create` creates an empty object with the prototype being the arg of create:
```js
var person = {
  firstName: 'Default',
  lastName: 'Default',
  greet: function() {
    return 'Hi ' + firstName;
  }
}

var evan = Object.create(person);
evan.firstName = 'Evan';
evan.lastName = 'Rolfe';
```

**ES6 Class**
- The class itself is an object! When you do `new`, you are creating new objects from the class object
- `class Employee extends Person` sets Person as prototype
