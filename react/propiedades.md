# Propiedades

La forma más sencilla de explicar las propiedades \(_props_\) de los componetes componentes sería decir que **funcionan de forma similar a los atributos HTML**. En otras palabras, **proporcionan valores de configuración para el componente**.  
Da igual si declaramos un componente como una función o una clase, nunca debe modificar sus propiedades.

Por ejemplo:

```js
function sum(a, b) {
  return a + b;
}
```

Estas funciones se llaman _puras_ - que [es un concepto de la programación funcional](https://es.wikipedia.org/wiki/Programaci%C3%B3n_funcional#Funciones_puras) - porque no intentan cambiar sus entradas, y siempre devuelven el mismo resultado para las mismas entradas.

Por el contrario, esta función es _impura_ porque cambia su propia entrada:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React es bastante flexible pero tiene **una sola regla estricta**:

> Todos los componentes React deben actuar como funciones puras con respecto a sus propiedades.

En motores ES5 no será capaz de mutar `this.props` porque está congelado \(es decir `Object.isFrozen(this.props) === true;`

