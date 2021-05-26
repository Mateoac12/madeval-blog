---
title: "Agregar Lint a Un Proyecto"
description: ""
lead: ""
date: 2021-05-25T15:18:33-03:00
lastmod: 2021-05-25T15:18:33-03:00
draft: false
weight: 50
images: ["agregar-lint-a-un-proyecto.jpg"]
contributors: []
---

## Incluir Eslint a un proyecto

Para instalar Eslint es necesario tener un proyecto en Node, la instalacion sera con `npm i eslint -D`, (ya que es una dependencia de desarrollo). Y luego para configurarla debemos escirbir `npx eslint --init`. Hau un fallo a la hora de especificar en donde se va a desplegar, y aun asi al colocar Node se aplica Browser, para eso hay que hacerlo luego manualmente agregando al archivo que se genero `"node": true`. Una configuracion que tambien me gusta es el espaciado de las tabulaciones, por lo general me gusta en 2, asi que tambien lo suelo cambiar.

Ahora bien, hay un estandar que me gusta mucho usar llamado [JavaScript Standar Style](https://standardjs.com/). Para instalarlo (en caso de que no desees usar una configuracion manual de eslint como la que hicimos), debemos ejecutar `npm i standard -D`. Y para aplicar el eslin que tiene por defecto a su proyecto debemos irnos al `package.json` y agregarle lo siguiente:

```javascript
...
,
  "eslintConfig": {
    "extends": "./node_modules/standard/eslintrc.json"
  }
```

Para detectar los errores hay una manera mas visual con una extension de VSCode y otra por consola. La visual es descargando la extension de ES-lint y Error Lends. La de consola se puede hacer agregando un script que se llame `"lint": "eslint ."`

Ya con esto podremos escribir un codigo mas limpio!.

😁