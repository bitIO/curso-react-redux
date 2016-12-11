# Despliegue

_Esta sección entra más dentro de las responsabilidades de webpack. Configurar webpack requiere un curso por si mismo por lo que aquí sólo se monstrará una pincelada_

Lo primero que tenemos que tener en cuenta es que tendremos dos _pipelines_. Una para desarrollar y otra para desplegar. En este caso, vamos a usar Heroku como servidor.

![](/assets/Lineas de desarrollo.png)


## Usando create-react-app

Heroku tiene ya implementado el proceso de despliegue para aplicaciones creadas usando `create-react-app`.

El proceso sería

```
npm install -g create-react-app
create-react-app my-app
cd my-app
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
```