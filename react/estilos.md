# Estilos

Pff, hay tantas formas de implementar los estilos en react y ningua se ha impuesto.

## Inline

Deshacerse de los estilos en línea fue una de las principales razones para el uso de archivos CSS separados en el primer lugar. Ahora estamos de vuelta allí

```js
render(props, context) {
  const notes = this.props.notes;
  const style = {
    margin: '0.5em',
    paddingLeft: 0,
    listStyle: 'none'
  };

  return <ul style={style}>{notes.map(this.renderNote)}</ul>;
}
```

Es una solución muy básica.


## Radium

Lo más importante de Radium es que proporciona abstracciones necesarias para tratar las media queries y pseudo clases (por ejemplo,: hover). Tendremos que anotar nuestras clases con `@Radium``

```js
const styles = {
  button: {
    padding: '1em',

    ':hover': {
      border: '1px solid black'
    },

    '@media (max-width: 200px)': {
      width: '100%',

      ':hover': {
        background: 'white',
      }
    }
  },
  primary: { background: 'green' },
  warning: { background: 'yellow' },
};
...
<button style={[styles.button, styles.primary]}>Confirm</button>
```

## CSS Modules

CSS Modules parten de la premisa de que las reglas CSS deben ser locales por defecto. Si usted hace difícil utilizar globales, usted maneja solucionar el problema más grande de CSS. El enfoque todavía nos permite desarrollar CSS como hemos estado acostumbrados. Esta vez estamos operando en un contexto local más seguro, por defecto.
Esto en sí resuelve una gran cantidad de problemas que las bibliotecas tratan de resolver en sus propias maneras. Si necesitamos estilos globales, todavía podemos obtenerlos. Aún así, es posible que desee tener algunos en torno a algunos de alto nivel de estilo, después de todo. Esta vez estamos siendo explícitos al respecto.