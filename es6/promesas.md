# Promesas

A cualquier desarrollador de JS seguro que le suena esto

```js
llamada1(function(valor1) {
  llamada2(valor1, function(valor2) {
    llamada3(valor2, function(valor3) {
      llamada4(valor3, function(valor4) {
        // aquí ya, si eso, hacemos algo con ello
      });
    });
  });
});
```

El uso de _callbacks_ acarrea una serie de problemas - son difíciles de componer y gestionar. Las **Promesas** actuan como proxy para valores que *aún no están disponibles*.
Una Promesa puede estar en uno de estos estados:

* pending ( pendiente ): estado inicial, no cumplida o rechazada.
* fulfilled ( cumplida ): operación satisfactoria.
* rejected ( rechazada ): operación fallida.

```js
'use strict';
var promiseCount = 0;

function testPromise() {
  var thisPromiseCount = ++promiseCount;

  var log = document.getElementById('log');
  log.insertAdjacentHTML('beforeend', thisPromiseCount + 
      ') Started (<small>Código sincrónico empezado</small>)<br/>');

  // Hacemos una nueva promesa: prometemos la cadena 'resultado' (después de esperar 3s)
  var p1 = new Promise(
    // La función de resolución se llama con la capacidad de  
    // resolver o rechzar la promesa
    function(resolve, reject) {       
      log.insertAdjacentHTML('beforeend', thisPromiseCount + 
          ') Promesa empezada (<small>Código asíncrono empezado</small>)<br/>');
      // Esto sólo es un ejemplo para crear asincronismo
      window.setTimeout(
        function() {
          // ¡Cumplimos la promesa!
          resolve(thisPromiseCount)
        }, Math.random() * 2000 + 1000);
    });

  // definimos que hacer cuando la promesa se ha cumplido.
  p1.then(
    // Solo registramos el mensaje y un valor.
    function(val) {
      log.insertAdjacentHTML('beforeend', val +
          ') Promesa cumplida (<small>Código asíncrono terminado</small>)<br/>');
    });

  log.insertAdjacentHTML('beforeend', thisPromiseCount + 
      ') Promesa hecha (<small>Código sincrónico terminado</small>)<br/>');
}
```

Otro ejemplo usando XMLHttpRequest

```js
'use strict';

// A-> la función $http se implementa con el fin de seguir el modelo de Adapter estándar
function $http(url){
 
  // Un pequeño ejemplo de objeto
  var core = {

    // Método que realiza la petición AJAX
    ajax : function (method, url, args) {

      // Creando una promesa.
      var promise = new Promise( function (resolve, reject) {

        // Instancia XMLHttpRequest
        var client = new XMLHttpRequest();
        var uri = url;

        if (args && (method === 'POST' || method === 'PUT')) {
          uri += '?';
          var argcount = 0;
          for (var key in args) {
            if (args.hasOwnProperty(key)) {
              if (argcount++) {
                uri += '&';
              }
              uri += encodeURIComponent(key) + '=' + encodeURIComponent(args[key]);
            }
          }
        }

        client.open(method, uri);
        client.send();

        client.onload = function () {
          if (this.status == 200) {
            // Realiza la función "resolver" cuando this.status es igual a 200
            resolve(this.response);
          } else {
            // Realiza la función "rechazar" cuando this.status es diferente de 200
            reject(this.statusText);
          }
        };
        client.onerror = function () {
          reject(this.statusText);
        };
      });

      // Return the promise
      return promise;
    }
  };

  // Patrón Adapter
  return {
    'get' : function(args) {
      return core.ajax('GET', url, args);
    },
    'post' : function(args) {
      return core.ajax('POST', url, args);
    },
    'put' : function(args) {
      return core.ajax('PUT', url, args);
    },
    'delete' : function(args) {
      return core.ajax('DELETE', url, args);
    }
  };
};
// Fin A

// B-> Aquí se definen sus funciones y su playload
var mdnAPI = 'https://developer.mozilla.org/en-US/search.json';
var payload = {
  'topic' : 'js',
  'q'     : 'Promise'
};

var callback = {
  success : function(data){
     console.log(1, 'success', JSON.parse(data));
  },
  error : function(data){
     console.log(2, 'error', JSON.parse(data));
  }
};
// Fin B

// Ejecutra la llamada al método
$http(mdnAPI)
  .get(payload)
  .then(callback.success)
  .catch(callback.error);
```

Otro ejemplo usando promesas para cargar imágenes

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="viewport" content="width=device-width">

    <title>Promise example</title>

    <link rel="stylesheet" href="">
    <!--[if lt IE 9]>
      <script src="//html5shiv.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->
  </head>

  <body>
    <h1>Promise example</h1>

    <p>Darth Vader image by <a href="https://www.flickr.com/photos/digital_stability/">Shawn Taylor</a>, published under a <a href="https://creativecommons.org/licenses/by-nc-nd/2.0/">Attribution-NonCommercial-NoDerivs 2.0 Generic</a> license.</p>
  </body>

  <script>
  function imgLoad(url) {
    // Create new promise with the Promise() constructor;
    // This has as its argument a function
    // with two parameters, resolve and reject
    return new Promise(function(resolve, reject) {
      // Standard XHR to load an image
      var request = new XMLHttpRequest();
      request.open('GET', url);
      request.responseType = 'blob';
      // When the request loads, check whether it was successful
      request.onload = function() {
        if (request.status === 200) {
        // If successful, resolve the promise by passing back the request response
          resolve(request.response);
        } else {
        // If it fails, reject the promise with a error message
          reject(Error('Image didn\'t load successfully; error code:' + request.statusText));
        }
      };
      request.onerror = function() {
      // Also deal with the case when the entire request fails to begin with
      // This is probably a network error, so reject the promise with an appropriate message
          reject(Error('There was a network error.'));
      };
      // Send the request
      request.send();
    });
  }
  // Get a reference to the body element, and create a new image object
  var body = document.querySelector('body');
  var myImage = new Image();
  // Call the function with the URL we want to load, but then chain the
  // promise then() method on to the end of it. This contains two callbacks
  imgLoad('myLittleVader.jpg').then(function(response) {
    // The first runs when the promise resolves, with the request.reponse
    // specified within the resolve() method.
    var imageURL = window.URL.createObjectURL(response);
    myImage.src = imageURL;
    body.appendChild(myImage);
    // The second runs when the promise
    // is rejected, and logs the Error specified with the reject() method.
  }, function(Error) {
    console.log(Error);
  });
  </script>
</html>
```