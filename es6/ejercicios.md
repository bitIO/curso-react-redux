# 1.10 - Ejercicios

## Clases

Vuelve a escribir estas dos tipos de objeto para utilizar la palabra clave `class`, en lugar de manipular el prototipo directamente. `Speaker` es un tipo simple que expone un método `speak` que, cuando se llama, registra el texto dado junto con el nombre del *speaker*. `Shouter` es un subtipo de Speaker que grita su texto y lo convierte en mayúsculas.

```js
function Speaker(name, verb) {
  this.name = name
  this.verb = verb || "says"
}
Speaker.prototype.speak = function(text) {
  console.log(this.name + " " + this.verb + " '" + text + "'")
}

function Shouter(name) {
  Speaker.call(this, name, "shouts")
}
Shouter.prototype = Object.create(Speaker.prototype)
Shouter.prototype.speak = function(text) {
  Speaker.prototype.speak.call(this, text.toUpperCase())
}

new Shouter("Francisco Calle").speak("Hola ES6")
```

## Get / Set

Modifica la caja registradora para que no se pueda poner un valor que no sea un integer positivo y que cuando imprimamos el balance, si estamos por debajo de 0 nos avise.

```js
var register = {
  balance: 0,
  deposit: function(value){
    this.balance += value
  },
  withdraw: function(value){
    this.balance -= value
  }
}

register.balance = 100000

register.deposit(1)
register.deposit("0")
register.deposit("00000")
register.balance
```


Más sitios donde aprender

http://es6katas.org/
https://babeljs.io/learn-es2015/
http://stack.formidable.com/es6-interactive-guide/#/
https://egghead.io/technologies/es6
https://hacks.mozilla.org/category/es6-in-depth/