# Estructura de un proyecto

React no impone ninguna estructura de proyecto en particular. Lo bueno de esto es que permite hacer una estructura para satisfacer nuestras necesidades. Lo malo es que no es posible proporcionar una estructura ideal que funcione para cada proyecto.

## Directorio por concepto

```
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

```
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

```
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