# Redux con React

**Redux no tiene relación con React**. Puedes escribir aplicaciones Redux con React, Angular, Ember, jQuery o JavaScript puro.

Dicho esto, Redux funciona especialmente bien con bibliotecas como React y Deku porque te permiten describir la interfaz de usuario como una función de estado, y Redux emite actualizaciones de estado en respuesta a las acciones.

Para poder usar Redux con React necesitamos instalar el paquete npm `npm install --save react-redux.`

## Cambio de paradigma

A partir de ahora, tendremos dos tipos de componentes, los componetes de representación y los componentes contenedores - más detalles en [este artículo en medium de Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0).

<table>
<thead>
  <tr>
    <th></th>
    <th>Componentes de presentación</th>
    <th>Componentes contenedor</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Propósito</td>
    <td>Cómo se ven las cosas (marcado, estilos)</td>
    <td>Cómo funcionan las cosas (búsqueda de datos, actualizaciones de estado)</td>
  </tr>
  <tr>
   <td></td>
   <td></td>
   <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
<tbody>
</table>


|  | |  |
|Consciente de Redux| No | Sí |
| Lectura de datos | Lee de props | Se suscribe al estado Redux |
| Modificar datos | Callbacks en props | Dispatch acciones Redux |