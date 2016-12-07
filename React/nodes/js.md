## Nodos usando Javascript

Al lio directamente 😎

```js
const reactNodeLi = React.createElement('li', {id:'li1'}, 'one');
````

El primer argumento define *qué elemento HTML* queremos representar. El segundo argumento define *los atributos* en el elemento HTML. Y, el tercer argumento define *cuál será el nodo dentro del elemento*.

Suponiendo que partimos de un código HTML así

```html
<ul id="app"></ul>
```

podemos insertar de manera dinámica un nodo

```js
ReactDOM.render(reactNodeLi, document.getElementById('app'));
```

Haciéndolo un poco más complejo

```js
var rElmLi1 = React.createElement('li', {id:'li1'}, 'one');
var rElmLi2 = React.createElement('li', {id:'li2'}, 'two');
var rElmLi3 = React.createElement('li', {id:'li3'}, 'three');

//create <ul> React element and add child React <li> elements to it
var reactElementUl = React.createElement('ul', {className:'myList'}, rElmLi1,rElmLi2,rElmLi3);
```

renderizariamos un HTML

```html
<ul class="myList" data-reactid=".0">
    <li id="li1" data-reactid=".0.0">one</li>
    <li id="li2" data-reactid=".0.1">two</li>
    <li id="li3" data-reactid=".0.2">three</li>
</ul>
```

La lista de los elementos html que soporta React es
```
a abbr address area article aside audio b base bdi bdo big blockquote body br
button canvas caption cite code col colgroup data datalist dd del details dfn
dialog div dl dt em embed fieldset figcaption figure footer form h1 h2 h3 h4 h5
h6 head header hgroup hr html i iframe img input ins kbd keygen label legend li
link main map mark menu menuitem meta meter nav noscript object ol optgroup
option output p param picture pre progress q rp rt ruby s samp script section
select small source span strong style sub summary sup table tbody td textarea
tfoot th thead time title tr track u ul var video wbr
```

### Atributos
El segundo argumento que se pasa a `React.createElement (type, props, children)` es un objeto que contiene las propiedades del elemento (`props`) que tienen varias funciones:

* Las propiedades pueden convertirse en atributos HTML. Si un prop coincide con un atributo HTML conocido, se agregará al elemento HTML final en el DOM.
* Las propiedades pasados a `createElement()` se convierten en valores almacenados en un objeto `prop` como una propiedad de la instancia creada. Ees decir, [INSTANCIA].props.[NOMBRE DEL PROPIEDAD]). Los accesorios de ampliación se utilizan para introducir valores en los componentes.
* Algunos propiedades especiales tienen efectos secundarios (por ejemplo `key`, `ref`, y `dangerouslySetInnerHTML`)

Se puede pensar en los propiedades como las opciones de configuración para los nodos React y en otro sentido puedes pensar en ellos como atributos HTML.

```js
var reactNodeLi = React.createElement('li',
{
    foo:'bar',
    id:'li1',
    // no se puede usar class por ser una palabra reservada por
    // lo que se usa className para el atributo class del nodo
    className:'blue',
    'data-test':'test',
    'aria-test':'test',
    // las propiedades de estilo se pasarán dentro de un objeto
    // y en formato como camel case
    style:{backgroundColor:'red'}
},
'text'
);
```
 el resultado sería
 
 ```html
 <li id="li1" data-test="test" class="blue" aria-test="test" style="background-color:red;" data-reactid=".0">text</li>
 ```

**Notas**

* Dejar un atributo vacio lo convierte en `true` (id="" se convierte en id="true")
* Si un propiedad se duplica, el último definido gana.
* Si pasa atributos a elementos HTML nativos que no existen en la especificación HTML, React no los procesará. Sin embargo, si utiliza un elemento personalizado (es decir, no un elemento HTML estándar), los atributos personalizados se agregarán a elementos personalizados (por ejemplo, <drago-component custom-attribute = "foo" />).
* El atributo de clase debe escribirse className
* El atributo for debe escribirse htmlFor
* El atributo `style` a de ser un objeto con las propiedades en _camel case_.
* Los elementos de formulario HTML (por ejemplo, <input />, <textarea />, etc.) creados como nodos React admiten algunos atributos que son afectados por la interacción del usuario (value, checked, selected).
* Los atributos `ref` y `dangerouslySetInnerHTML` - que no existen en DOM - son de uso para acceder a un nodo y para ...
* Todos los atributos son camel case (por ejemplo, accept-charset se escribe como acceptCharset) que difiere de cómo se escriben en HTML.

Los siguientes son los atributos HTML que React soporta (mostrados en camel case):

```
accept acceptCharset accessKey action allowFullScreen allowTransparency alt
async autoComplete autoFocus autoPlay capture cellPadding cellSpacing challenge
charSet checked classID className colSpan cols content contentEditable
contextMenu controls coords crossOrigin data dateTime default defer dir
disabled download draggable encType form formAction formEncType formMethod
formNoValidate formTarget frameBorder headers height hidden high href hrefLang
htmlFor httpEquiv icon id inputMode integrity is keyParams keyType kind label
lang list loop low manifest marginHeight marginWidth max maxLength media
mediaGroup method min minLength multiple muted name noValidate nonce open
optimum pattern placeholder poster preload radioGroup readOnly rel required
reversed role rowSpan rows sandbox scope scoped scrolling seamless selected
shape size sizes span spellCheck src srcDoc srcLang srcSet start step style
summary tabIndex target title type useMap value width wmode wrap
```

### Factorias

React proporciona una forma para crear nodos de elementos HTML comúnmente utilizados: las factorias. Estas son simplemente funciones que generan un `ReactElement`. Si usamos `JSX`es bastante probable que jamás usemos las factorias. 
React también proporciona una forma de crear factorías propias por medio de `React.createFactory(type)`.

```js
var reactNodeLi = React.DOM.li({id:'li1'}, 'one');
var reactNodeLi = React.createElement('li', {id:'li1'}, 'one');
```

Estos son los elementos que tienen factoria

```
a,abbr,address,area,article,aside,audio,b,base,bdi,bdo,big,blockquote,body,br,button,
canvas,caption,cite,code,col,colgroup,data,datalist,dd,del,details,dfn,dialog,div,dl,
dt,em,embed,fieldset,figcaption,figure,footer,form,h1,h2,h3,h4,h5,h6,head,header,hgroup,
hr,html,i,iframe,img,input,ins,kbd,keygen,label,legend,li,link,main,map,mark,menu,
menuitem,meta,meter,nav,noscript,object,ol,optgroup,option,output,p,param,picture,
pre,progress,q,rp,rt,ruby,s,samp,script,section,select,small,source,span,strong,
style,sub,summary,sup,table,tbody,td,textarea,tfoot,th,thead,time,title,tr,track,
u,ul,var,video,wbr,circle,clipPath,defs,ellipse,g,image,line,linearGradient,mask,
path,pattern,polygon,polyline,radialGradient,rect,stop,svg,text,tspa
```

### Eventos

React crea lo que llama `SyntheticEvent` para cada evento, que contiene los detalles del evento similar a los detalles que se definen para los eventos DOM. React _normaliza_ los eventos **para que se comporten de manera consistentem en los diferentes navegadores**. La instancia `SyntheticEvent`, para un evento, se pasa a los eventos:

```js
const mouseOverHandler = () => {
  console.log('estás encima de mi!');
};
const clickhandler = () => {
  console.log('me pinchas y no sangro');
};
const reactNode = React.createElement(
  'div',
  { onClick: clickhandler, onMouseOver: mouseOverHandler },
  'click y mouseover'
);
ReactDOM.render(reactNode, document.getElementById('app'));
```

Cada instancia de `SyntheticEvent` tiene las siguientes propriedades 
```
bubbles
cancelable
currentTarget - DOMEventTarget
defaultPrevented
eventPhase
isTrusted
nativeEvent - DOMEvent
preventDefault
isDefaultPrevented
stopPropagation
isPropagationStopped()
target - DOMEventTarget
timeStamp
type
```

Si necesitamos los detalles del evento del explorador, se puede acceder a él utilizando la propiedad `nativeEvent` que se encuentra en el objeto `SyntheticEvent` que se pasa al handler del evento de React.