# var, let, const

`var` tiene _scope_ en la función.

```js
function dividir(x, y) {
  if (y !== 0) {
    var result;
    result = x / y;
  }
  return result;
}
console.log( 'resultado: ' + dividir(10, 2) );
```

Este tipo de _errores_ se solucionan usando `let`
```js
function dividir(x, y) {
  if (y !== 0) {
    let result;
    result = x / y;
  }
  return result; // peta - result is not defined
}
console.log( 'resultado: ' + dividir(10, 2) );
```

Para valores fijos, usaremos `const` - que tiene _scope_ de bloque - ya que no pueden ser reasignados (pero si modificados).

```js
const PI = 3.141592;
const COLOR = {
  nombre: 'Rojo',
  hex: '#FF0000'
};
// peta - "COLOR" is read-only
COLOR = {
  nombre: 'GRIS',
  hex: '#CCC'
};

// pero esto si es válido
COLOR.nombre = 'GRIS';
COLOR.hex = '#CCC';
```