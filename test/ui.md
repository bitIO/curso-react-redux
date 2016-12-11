# UI Testing

Dado que React es _**sólo**_ una capa de vista, es importante asegurarse que esta es correcta. Para aseguranos de esto, usaremos 

## Test estructurales

Supongamos que tenemos una pantalla que login de facebook así

![](/assets/login-facebook.png)

Aquí nos enfocaremos en la estructura de la interfaz de usuario y en cómo está diseñada. Para las pruebas estructurales, estamos probando si tiene o no el siguiente contenido:

* Un título con "Iniciar sesión en Facebook"
* Dos entradas para el nombre de usuario y la contraseña.
* Un botón de envío.
* Una pantalla de error para mostrar errores.

Para React, podemos usar `Enzyme` como una forma de hacer pruebas estructurales, pero ahora también podemos usar snapshots testing de `Jest` para hacer las cosas aún más simples.

## Test de interacción

El interfaz de usuario todo trata de interactuar con el usuario. Hacemos esto con un montón de elementos de la interfaz de usuario, como botones, vínculos y elementos de entrada. Con las pruebas de interacción, necesitamos probar si funcionan correctamente.

Vamos a utilizar de nuevo el componente de inicio de sesión anterior como un ejemplo. Debe hacer estas cosas:

* Cuando hacemos clic en el botón Enviar, debe darnos el nombre de usuario y la contraseña.
* Cuando hacemos clic en el enlace "Cuenta olvidada", debe redirigir a una nueva página.

Tenemos pocas maneras de hacer este tipo de pruebas con React. La forma más sencilla es usar Enzyme.

## Tes de estilos

La interfaz de usuario tiene que ver con estilos (por muy feos que estos puedan ser, deben ser probados). Con las pruebas de estilo, estamos evaluando la apariencia de nuestros componentes de interfaz de usuario entre los cambios de código. Este es un tema bastante complejo y usualmente lo hacemos comparando imágenes.

Si estamos usando estilos en línea todo el camino, podemos usar los snapshosts de Jest. Pero para obtener resultados aún mejores, debemos considerar el uso de herramientas tales como:

* BackstopJS
* PhantomCSS
* Gemini
* Happo