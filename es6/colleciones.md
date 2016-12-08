# Colecciones

## Map

El objeto Map es un sencillo mapa clave/valor. Cualquier valor (tanto objetos como valores primitivos) pueden ser usados como clave o valor. Un objeto Map puede iterar sobre sus elementos en orden de inserción. Un bucle for..of devolverá un array de [clave, valor] en cada iteración.

```js
var myMap = new Map();

var keyObj = {},
    keyFunc = function () {},
    keyString = "a string";

// setting the values
myMap.set(keyString, "value associated with 'a string'");
myMap.set(keyObj, "value associated with keyObj");
myMap.set(keyFunc, "value associated with keyFunc");

myMap.size; // 3

// getting the values
myMap.get(keyString);    // "value associated with 'a string'"
myMap.get(keyObj);       // "value associated with keyObj"
myMap.get(keyFunc);      // "value associated with keyFunc"

myMap.get("a string");   // "value associated with 'a string'"
                         // because keyString === 'a string'
myMap.get({});           // undefined, because keyObj !== {}
myMap.get(function() {}) // undefined, because keyFunc !== function () {}
```

```js
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");
for (var [key, value] of myMap) {
  alert(key + " = " + value);
}
// Mostrará 2 alertas; primero con "0 = zero" y segundo con "1 = one"

for (var key of myMap.keys()) {
  alert(key);
}
// 2 alertas; la primera con "0" y la segunda "1"

for (var value of myMap.values()) {
  alert(value);
}
// 2 alertas; la primera con "zero" y la segunda con "one"

for (var [key, value] of myMap.entries()) {
  alert(key + " = " + value);
}
// 2 alertas; la primera con "0 = zero" y la segunda con "1 = one"

myMap.forEach(function(value, key, myMap) {
  alert(key + " = " + value);
})
// 2 alerts; la primera con "0 = zero" y la segunda con "1 = one"
```

## WeakMap

Es similar al map. **Las claves deben ser objetos** y no previene que la claves sean limpiadas por el `garbage collector`. Es decir, que puede ser últil para prevenir fugas de memoria. Ah, y también puede ser iterado.

## Set

Los objetos Set son colecciones de valores, se puede iterar sus elementos en el orden de su inserción. Un valor en un Set sólo puede estar una vez; éste es único en la colección Set. Ya que cada valor en el Set tiene que ser único, la igualdad del valor será comprobada y esta igualdad no se basa en el mismo algoritmo usado en el operador `===`. Específicamente, para Sets, +0 (el cual es estrictamente igual a -0) y -0 son valores distintos. Esto ha cambiado en la última especificación ECMAScript 6. 

```js
var mySet = new Set();

mySet.add(1);
mySet.add(5);
mySet.add("some text");
var o = {a: 1, b: 2};
mySet.add(o);

mySet.has(1); // true
mySet.has(3); // false, 3 no ha sido añadido al Set
mySet.has(5);              // true
mySet.has(Math.sqrt(25));  // true
mySet.has("Some Text".toLowerCase()); // true
mySet.has(o); // true

mySet.size; // 4

mySet.delete(5); // Elimina 5 del Set
mySet.has(5);    // false, 5 fue eliminado

mySet.size; // 3, sólo removimos un valor
```

Entonces, esto, que longitud tendrá?

```js
var mySet = new Set();

mySet.add(-0);
mySet.add(+0);

for(let item of mySet) console.log(item);
console.log(mySet.size);
```

**Pro tip**
```js
Set.prototype.isSuperset = function(subset) {
    for (var elem of subset) {
        if (!this.has(elem)) {
            return false;
        }
    }
    return true;
}

Set.prototype.union = function(setB) {
    var union = new Set(this);
    for (var elem of setB) {
        union.add(elem);
    }
    return union;
}

Set.prototype.intersection = function(setB) {
    var intersection = new Set();
    for (var elem of setB) {
        if (this.has(elem)) {
            intersection.add(elem);
        }
    }
    return intersection;
}

Set.prototype.difference = function(setB) {
    var difference = new Set(this);
    for (var elem of setB) {
        difference.delete(elem);
    }
    return difference;
}

//Examples
var setA = new Set([1,2,3,4]),
    setB = new Set([2,3]),
    setC = new Set([3,4,5,6]);

setA.isSuperset(setB); // => true
setA.union(setC); // => Set [1, 2, 3, 4, 5, 6]
setA.intersection(setC); // => Set [3, 4]
setA.difference(setC); // => Set [1, 2]

// es decir, que podemos hacer cosas para las 
// que antes usábamos lodash o underscore
```

## WeakSet
A ver si lo adivinas? 😝

Pues casi, porque **no puede ser iterado!**

```js
var ws = new WeakSet();
var obj = {};
var foo = {};

ws.add(window);
ws.add(obj);

ws.has(window); // true
ws.has(foo);    // false, foo has not been added to the set

ws.delete(window); // removes window from the set
ws.has(window);    // false, window has been remove
```