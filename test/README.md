# Testing

Tenemos que pensar por qué tenemos que probar. Hay muchas razones: 

* Para encontrar bugs.
* Para asegurarse de que las cosas no se rompen con cada entrega nueva de código.
* Para mantener las pruebas como documentación viva.
* Es especialmente importante cuando se trabaja con equipos, ya que permite a diferentes personas contribuir con garantias.

Veremos diferentes tipos de test en React: Test de UI y test de Redux

## Glosario

### [Enzyme](http://airbnb.io/enzyme/)

Enzyme es una utilidad de prueba de JavaScript para React que hace más fácil la verificación, manipulación y navegación de la salida de nuestro ccomponents.

### [React Test Tool](http://facebook.github.io/react/docs/test-utils.html)

Utilidades de prueba para React usadas por Enzyme.

### [Shallow rendering](http://facebook.github.io/react/docs/test-utils.html)

La renderización superficial permite crear instancias de un componente y obtener eficazmente el resultado de su método de render a sólo un nivel profundo en lugar de representar componentes de forma recursiva en un DOM. La representación superficial es útil para las pruebas unitr, donde se prueba un componente en particular solamente (no sus hijos). Esto también significa que el cambio de un componente secundario no afectará las pruebas para el componente principal. La prueba de un componente y de todos sus hijos se puede lograr con el método [`mount()`](http://airbnb.io/enzyme/docs/api/mount.html) de _Enzyme_, también conocido como `DOM full rendering`.