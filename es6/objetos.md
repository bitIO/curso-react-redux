## Object literals
En ES5 podíamos crear un objeto de la siguiente manera:

```js
var nombre = "Francisco";
var apellidos = "Calle Moreno";
var twitter = "@bit_jammer";

var profesor = {
  nombre: nombre,
  apellidos: apellidos,
  twitter:twitter
};
```

En ES6 podemos usar el método abreviado:

```js
const nombre = "Francisco";
const apellidos = "Calle Moreno";
const twitter = "@bit_jammer";
const profesor = { nombre, apellido, twitter };
```

## Destructuring

Nos permite extraer datos de array u objectos usando una sintaxis que mapee la construción del objeto 😐

```js
const colores = ['rojo', 'verde', 'azul'];

// versión ES5
var primario = colors[0];
var secundario = colors[1];
var terciario = colors[2];
console.log( primario, secundario, terciario );

// versión ES6
const [primario, secundario, terciario] = colores;
console.log( primario, secundario, terciario );
```

## Spread operator

```js
const colores = ['rojo', 'verde', 'azul', 'amarillo', 'naranja'];
const [primario, secundario, ...resto] = colores;
console.log( primario, secundario, resto );
```

## Funciones con retorno de muliples valores

```js
function fecha() {
  var d = new Date();
  return [d.getDate(), d.getMonth() + 1, d.getFullYear()];
}
var [dia, mes, anio] = fecha();
console.log(dia, mes, anio);
```

## Creando variables a partir de objetos

En ES5
```js
var profesor = {
  nombre: 'Francisco',
  apellidos: 'Calle Moreno',
  twitter: '@bit_jammer'
};
var nombre = profesor.nombre;
var apellidos = profesor.apellidos;
var twitter = profesor.twitter;
console.log(nombre, apellidos, twitter);
```

En ES6
```js
var { nombre, apellidos, twitter } = profesor;
console.log(nombre, apellidos, twitter);
```