---
layout: post
title: Cómo empezar con Git
tags: git recursos cursos
---
## [**Aprende Javascript con MentoringJS - Pretraining Step 2**](http://MentoringJS.com)

**Git** es un _software de control de versiones_ libre y de código abierto diseñado para tratar cualquier proyecto, sea pequeño o grande, con velocidad y eficiencia.
Esta es la propia definición que aparece en la página oficial de [Git](https://git-scm.com).

>Un sistema de control de versiones es una aplicación que permite a diferentes programadores trabajar en un proyecto común ocupándose cada uno de una parte y con un control centralizado. Es ideal para grandes proyectos o también para trabajar con otras personas que se puedan encontrar lejos físicamente.

Para empezar con Git hay que descargarse el programa. Este se puede descargar desde su [página oficial](https://git-scm.com) dónde se puede elegir la plataforma dónde lo vas a instalar.

Antes de comenzar a trabajar con Git es recomendable crearse una cuenta en [Github](https://github.com).
>Github es, según la definición en su web, una gran comunidad de programadores que crea la posibilidad de descubrir, compartir y crear mejor software. Desde proyectos de código abierto hasta repositorios de equipos privados, son la plataforma de todos para el desarrollo colaborativo.

Una vez se tenga una cuenta en Github se podrá trabajar directamente con Git en tu dispositivo y sincronizar tus trabajos directamente en tu cuenta de Github.

Para empezar a aprender Git hay muchos recursos en Internet. Los que yo estoy utilizando son los siguientes:

* [Curso de Coodeschool](https://www.codeschool.com/courses/try-git)
* [Curso Git](https://github.com/oslugr/curso-git) de la [Oficina de Software libre de la Universidad de Granada. OSLUGR](https://github.com/oslugr)
* [Libro Pro Git](https://git-scm.com/book/en/v2). Libro que publicitan en la misma web de Git

De estos recursos, el primero que hice fue el curso de **Codeschool**. Tiene una consola _virtual_ que te permite iniciarte sin necesidad de instalarte Git, ya que se puede hacer todo en su web. Está en inglés aunque se entiende perfectamente. Para empezar es muy recomendable.

Una vez te has instalado Git en tu dispositivo, yo recomendaría comenzar a leer el curso de la [OSLUGR](https://github.com/oslugr/curso-git) ya que es muy didáctico (es de una unviversidad). Empieza desde cero, desde la configuración del propio Git en tu dispositivo una vez te lo instalas. Te permite también realizar acciones como clonar repositorios de ejemplo (que al empezar en esto es más difícil saber cómo hacerlo). Este recurso es altamente recomendable para empezar.

>Este recurso es el que he tenido de soporte para hacer mis primeros pasos en Git y Github y para subir mis primeros proyectos (este markdown) a mi cuenta de Github.

Otra herramienta muy importante para trabajar con Github es el **Markdown**. Markdown es un lenguaje y también una herramienta de software que **convierte lenguaje en HTML válido.**

>Este archivo es un archivo Markdown ya que permite utilizar crear un arhivo de texto con recursos de HTML como subrayado, resaltado en negrita, enlaces, listas, tablas, etc., de una manera diferente al HTML tradicional con una sintaxis propia y más sencilla que el HTML. Si no te suena de nada el Markdown (como a mi cuándo lo oí por primera vez) hay diferentes recursos que pueden aclararte qué es y cómo usarlo:

1. [Una guía de Genbeta que detalla qué es y para qué sirve](https://www.genbeta.com/guia-de-inicio/que-es-markdown-para-que-sirve-y-como-usarlo)
1. [Tutorial de Markdown en inglés](https://masteringmarkdown.com/)


### **COMANDOS BÁSICOS GIT** ###

Para empezar con Git hay que conocer algunos comandos básicos. Los que a continuación pondré son un listado **resumido** de los comandos básicos. Están extraídos del curso de la [OSLUGR](https://github.com/oslugr/curso-git/blob/master/texto/uso_basico.md).

>Mi recomendación es que se revise el curso de la OSLUGR directamente para aprender en detalle y con ejemplos cómo utilizar Git.

_Iniciar_ un repositorio nuevo
```
git init
```

_Clonar_ un repositorio
```
git clone REPOSITORIO
```

_Preparar_ un archivo
```
git add NOMBRE_DEL_ARCHIVO
```

_Revisar_ como están las cosasç
```
git status
```

_Confirmar_ los cambios realizados
```
git commit NOMBRE_DEL_ARCHIVO
```

_Borrar_ un archivo del repositorio
```
git add -u
o
git commit -a
```

_Añadir_ un repositorio
```
git remote add ALIAS_DEL_REPOSITORIO DIRECCION_DEL_REPOSITORIO
```

_Eliminar_ un repositorio
```
git remote rm NOMBRE
```
_Cambiar_ el nombre
```
git remote rename NOMBRE_ANTERIOR NOMBRE_ACTUAL
```

 _Importar_ cambios desde un repositorio remoto
 ```
 git pull REPOSITORIO_REMOTO RAMA
 ```
 _Enviar_ cambios a un repositorio remoto
 ```
 git push REPOSITORIO_REMOTO RAMA
 ```
