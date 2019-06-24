# Section 3: Types and Operators

**Primitive Types**
- Primitive types: undefined, null, boolean, number, string, symbol (ES6)

**Operators**

**Coercion**
- 99% of the time use `===` over `==`
- `!==` over `!=`
- convert to boolean: `Boolean(null)`

**Existance and Boolean**
```js
a = 0;

if(a) {
  console.log("a is present");
}
```
- Does nothing because `Boolean(0) == false`

**Default Values**
```js
function greet(name) {
  name = name || 'Evan';
  console.log('Hello ' + name);
}

greet();
```


