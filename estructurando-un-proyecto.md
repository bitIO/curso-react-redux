# Estructurando un proyecto

React no impone ninguna estructura de proyecto en particular. Lo bueno de esto es que permite hacer una estructura para satisfacer nuestras necesidades. Lo malo es que no es posible proporcionar una estructura ideal que funcione para cada proyecto.

Sin embargo, existen una serie de starter kits que nos ayudan a la hora de generar un nuevo proyecto. En esta página [https://www.javascriptstuff.com/react-starter-projects/](https://www.javascriptstuff.com/react-starter-projects/) podéis encontrar multiples opciones. Mi opción preferida, es [reactboilerplate](https://github.com/react-boilerplate/react-boilerplate).

Si prefieres hacerlo a mano, por lo que sea 😛, puedes seguir estos _patrones_

## Directorio por concepto

```text
├── actions
│   ├── LaneActions.js
│   └── NoteActions.js
├── components
│   ├── App.jsx
│   ├── Editable.jsx
│   ├── Lane.jsx
│   ├── Lanes.jsx
│   ├── Note.jsx
│   └── Notes.jsx
├── constants
│   └── itemTypes.js
├── index.jsx
├── libs
│   ├── alt.js
│   ├── persist.js
│   └── storage.js
├── main.css
└── stores
    ├── LaneStore.js
    └── NoteStore.js
```

Es una estructura _tradicional_ pero que, en cuanto el proyecto crezca un poco, se nos quedará corta.

## Directorio por componente

```text
...
├── components
│   ├── App
│   │   ├── App.jsx
│   │   ├── app.css
│   │   ├── app_test.jsx
│   │   └── index.js
│   ├── Editable
│   │   ├── Editable.jsx
│   │   ├── editable.css
│   │   ├── editable_test.jsx
│   │   └── index.js
...
```

Hay algunos beneficios interesantes en este enfoque:

* Podemos aprovechar tecnologías como CSS Modules, para diseñar cada componente por separado.
* Dado que cada componente es un pequeño "paquete" propio, ahora sería más fácil extraerlos del proyecto. Podríamos mover los componentes genéricos en otro lugar y consumirlos en múltiples aplicaciones.
* Podemos definir pruebas unitarias a nivel de componentes. El enfoque le anima a probar. Todavía podemos tener pruebas de nivel superior alrededor del nivel raíz de la aplicación como antes.

## Directorio por vista

```text
├── index.jsx
├── main.css
└── views
    ├── Home
    │   ├── Home.jsx
    │   ├── home.css
    │   ├── home_test.jsx
    │   └── index.js
    ├── Register
    │   ├── Register.jsx
    │   ├── index.js
    │   ├── register.css
    │   └── register_test.jsx
    └── index.js
```

La idea es la misma que antes. La aplicación se inicia desde `index.jsx`, lo que activará las rutas que a su vez elige alguna vista para mostrar.

Esta estructura puede escalar más, pero incluso tiene sus límites. Una vez que el proyecto comienza a crecer, es posible que deseemos introducir nuevos conceptos a la misma. Podría ser natural introducir un concepto, como "feature", entre las vistas y los componentes.

Por ejemplo, es posible que tenga un LoginModal que se muestra en determinadas vistas si la sesión del usuario ha expirado. Se compone de componentes de nivel inferior. Una vez más, las características comunes podrían ser sacadas del propio proyecto en paquetes de sus propios como usted ve potencial para la reutilización.

