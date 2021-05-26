---
title: "Crear Server Con Heroku"
description: ""
lead: ""
date: 2021-05-25T14:11:54-03:00
lastmod: 2021-05-25T14:11:54-03:00
draft: false
weight: 50
images: ["crear-server-con-heroku.jpg"]
contributors: ["Madeval"]
tags: ["Node"]
---

>**Previo** a comenzar a realizar, el plan de estudio que se va a seguir en este articulo es el hecho por [Midudev](https://www.youtube.com/channel/UC8LeXCWOalN8SxlrPcG-PaQ) en su curso [Bootcamp FullStack Gratuito](https://www.youtube.com/playlist?list=PLV8x_i1fqBw0Kn_fBIZTa3wS_VZAqddX7). Desde iniciar un proyecto con node hasta desplegarlo con [Heroku](https://dashboard.heroku.com).


## Iniciar un proyecto con Node

Para comenzar a desarrollar, necesitamos dirigirnos al directorio en donde nos gustaria comenzar el proyecto. Una vez ahi abriremos la consola en ese directorio y escribiremos: `npm init -y`. Hay dos variantes aqui:
1. Utilizar el comando anterior para permitir que node complete automaticamente todas las opciones del proyecto (la que uso generalmente).
2. Utilizar el comando `npm init` y completar manualmente todas las opciones que node te permite al iniciar un proyecto.

>Ambas son validas, luego se podra modificar cualquiera de estas opciones. Es mas, no dentro de mucho estaremos tocando cosas aqui. En caso de queres especificar si un proyecto es de codigo abiero, mencionar a un autor, etc. Es aqui en donde se hace. En mi caso el proyectp sera de codigo abierto y sin ninguna mencion.

## Empezar a escribir codigo y agregar scripts para inicializar diferentes acciones

Una vez creado el proyecto con Node, crearemos un archivo llamado `index.js` en el directorio base (junto con el `package.json` que se acaba de crear).

Para no tenes que estar escribiendo continuamente `node index.js` en la consola para ejecurtar el codigo que esta ahi, iremos al `pabake.json` y en `scripts` agregaremos la opcion de `"start": "node index.js"`, tal como se ve en este ejemplo:

```javascript
  "scripts": {
    "dev": "nodemon index.js",
    "lint": "eslint .",
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

Esto aun asi trae una desventaja, y es que cada vez que hagamos un cambio, no se vera reflejado en el servidor. Para eso usaremos una libreria muy conocida llamada [nodemon](https://github.com/remy/nodemon), que nos va a permitir renderizar el codigo cada vez que hagamos un cambio.

#### Agregar nodemon y su script

Ahora bien, una vez que lo hayamos instalado con `npm install --save-dev nodemon` o `npm i nodemon -D` (esto es lo mismo, pero en una version mas simplificada), crearemos un script para no tener que ejecutar `nodemon index.js`. Para ello agregaremos un nuevo script llamado `"dev": "nodemon index.js"`. Aqui hay algo interesante y que se puede ver en varios proyectos y es el hecho de usar `"dev": ./node_modules/.bin/nodemon index.js`. En node no hace falta escribir esta primera ruta ya que es en donde busca por defecto, asi que si ves esto en otros scripts, saber que se puede remover y seguira funcionando correctamente.

## Escribir codigo en el index.js con express y crear la primera API

```javascript
const express = require('express')
const cors = require('cors')
const app = express()
/*  sirve para permitir a otros dominios consumir la api  */
app.use(cors())
/*  sirve para que se pueda hacer un post  */
app.use(express.json())

const PORT = 3001
// inicializa donde se va a desplegar el servidor
app.listen(PORT, () => {
  console.log(`server running on port : ${PORT}`)
})
```

#### Cors

Cors es una libreria que sirve para permitir (o restringir) el uso de la API en otros dominios, funcional en este caso para levantar los datos desde un front en otro puerto que no sea el 3001. Es un `middleware`, estos es una funcion que intercepta la peticion que esta pasando por la API. Para instalarla lo haremos con `npm i cors`, y luego lo usaremos con un `app.use(corse())` como se ve en el codigo superior. Esto lo que va a hacer es que, cuando el codigo se lea de manera descendente, va a entrar a ese middleware (ya que no tiene ruta especificada y eso genera que entre automaticamente). Ejecuta cors() y continua con su ejecicion al siguiente middleware de `express.json()`. Este anterior ya esta incorporado en las ultimas versiones de express, por lo que no hace falta instalarlo con npm. Permite que se puedan hacer peticiones post en la API detectando el formato de los archivos .json.

## Configurar la API y desplegarla en Heroku

Para poder deployar la API a Heroku es necesario iniciar un proyecto en git. Una vez que lo tengamos desplegado en GitHub debemos agregar un archivo llamado `Procfile`, este archivo se va a encargar de decirlos que tipo de recurso es el que queremos deployar y cual es el comando que precisa para inicializar el servicio:

```javascript
web: npm start
```

Luego de hacer esto, debemos instalar [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli), una vez instalado es momento de hacer las ultimas configuraciones en el codigo.

Si recordamos bien, estabamos usando un puerto local para levantar nuestro servidor, pero ahora esto a heroku no le sirve, ya que va a usar un puerto particular de el. Y para invocarlo debemos modificar eso de esta manera: 

```javascript
// esto lo que nos permite es acceder al puerto que va a asignar heroku internamente
// y en caso de no estar trabajando ahi, usar el 3001
const PORT = process.env.PORT || 3001

// es async, por lo que cuando termina de hacen la conexion, tira el console.log
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

## Deploy del proyecto a Heroku

Como primer paso debemos escribir en consola `heroku create`, de esta manera se generara una nueva url del proyecto. Y esta nueva url es interesante ya que proviene de Git, y lo que nos hizo fue crear un nuevo origen dentro de nuestro repositorio (ya por defecto es origin). Y este nuevo origen es el que vamos a tener que usar para deployar nuestras ramas a heroku.

Para eso podemos hacer un commit y a la hora de pushear hacemos `git push heroku main`.