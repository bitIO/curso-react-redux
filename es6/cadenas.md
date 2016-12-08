# Cadenas

## Template strings
Una _cadena_ de toda la vida

```js
var str = `Hola Francisco`;
console.log( str );
```

Ahora, las cadenas multi-linea, son _más sencillas_
```js
var multilinea = `
  Esta es una cadena
  en varias lineas
  sin hacer cosas raras
`;
console.log(multilinea);
```
## Sustitución

```js
var nombre = "Francisco";
console.log(`Hoy el profesor es ${nombre}`);

var profesor = {
  nombre: 'Francisco',
  apellidos: 'Calle Moreno',
  twitter: '@bit_jammer'
};
console.log(`El profesor es ${profesor.apellidos}, ${profesor.nombre}
```
## Expresiones

```js
console.log(`Hoy es ${new Date()}`);
var precio = 100;
var cambio = 0.89;
console.log(`El precio es dolares es ${ precio * cambio }$`);
```