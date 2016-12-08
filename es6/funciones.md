## Arrow functions

En ES5 teníamos 

```js
var saludo = function(mensaje, nombre) {
  return mesaje + ' ' + nombre;
}
console.log( saludo('Hola', 'Franciiiiiiisco') );
```

en ES6 lo podemos convertir en

```js
var saludo = (mensaje, nombre) => {
  return mesaje + ' ' + nombre;
}

// o, incluso más corto
var saludo = (mensaje, nombre) => mensaje + ' ' + nombre;

// o, si sólo tiene un parámetro
var saludo = nombre => 'Hola ' + nombre;

console.log( saludo('Hola', 'Franciiiiiiisco') );
```

El cambio no es sólo en lo visual, también ha cambiado el scope del _this_, lo que se conoce como _lexical this_.

```js
// ES5
// ------
var obj = {
  init: function() {
    var self = this;
    setTimeout(function() {
      self.doSomenthig();
    }, 1000);
  },
  doSomething: function() {
    console.log("doing something");
  }
};
obj.init();
```

```js
// ES6
// ------
var obj = {
  init: function() {
    setTimeout(() => this.doSomenthig(), 1000);
  },
  doSomething: function() {
    console.log("doing something");
  }
};
obj.init();
```

## Parámetros por defecto

Recuerdas haber hecho esto alguna vez?
```js
function saludo (nombre, mensaje) {
  if (mensaje === undefined) {
    mensaje = 'Hola';
  }
  console.log(saludo('Francisco');
}
```

Ahora ya no
```js
const saludo = (nombre, mensaje = 'Hola ') => mensaje + ' ' + nombre;
```