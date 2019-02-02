---
layout: post
title: Construir y desplegar un proyecto en React con Surge
tags: [npm, React, surge, deploy]
---
### [Aprende Javascript con MentoringJS - Step 11 ](http://MentoringJS.com)
Una vez he podido acabar el proyecto en React toca realizar el siguiente paso: desplegarlo en un sitio web. Para esto, existen muchas opciones en Internet, aunque yo me he decantado por hacerlo en [Surge](https://surge.sh/).

Siguiendo el artículo de [Joshua Tjhin en Medium](https://medium.com/@xueeey/build-and-automatically-deploy-a-react-site-4d5d6f07e4e8), he procedido a crear la producción de mi sitio web en React y, acto seguido, a desplegarlo en Surge.

Para ello, he seguido los pasos que se indica en dicho artículo:
```
npm install --global surge
npm run build   # Creates production build in build/
surge build     # Publishes
```

Primero se instala surge en el directorio dónde tienes el proyecto React. Después se crea la producción del sitio con _build_ y, por último, se publica.

En el último comando aparecerá una consola de Surge en la que se debe ir rellenado los pasos tal y como se indica en el vídeo de información que [tienen en su web](https://surge.sh/). O también como aparece en el artículo enlazado:

![Surge](https://cdn-images-1.medium.com/max/800/1*3-wRRgm0dGrCrW-ST2dVoQ.png "Surge")

En esta consola se pide un correo electrónico y una contraseña para registrarse. Después también te indica un dominio específico dónde se publicará el sitio. Este dominio se puede cambiar por uno personalizado, aunque siempre será un subdominio de surge.sh. Si se quiere añadir un dominio propio, se puede hacer, y hay indicaciones de cómo hacerlo en su web.

Una vez introducido el nuevo dominio automáticamente se publicará el sitio que hemos construido con _npm build_.

Posteriormente, si se quiere redesplegar el proyecto con más información se puede hacer de manera sencilla [añadiendo el dominio como CNAME en la carpeta del proyecto](https://surge.sh/help/remembering-a-domain) y ejecutando:
```
surge build ./
```

Bueno, todo esto lo he realizado y finalmente el resultado ha sido:

[Proyecto web Fisioterapia en React](http://felipefcor.surge.sh/)
