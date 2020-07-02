# ![texto alternativo](../master/applications/geoportal/logo.png "Oskari") Muestra de aplicación

Este es un ejemplo que puede ser usado como una plantilla para construir el frontend de la aplicación Oskari.

Da clíc en el botón de "Usar esta plantilla" en el repositorio para crear una copia de los archivos bajo tu nombre de usuario y empieza a personalizarlo.

Esta aplicación puede ser vista en http://dev.oskari.org. Para el backend mira https://github.com/oskariorg/sample-server-extension.

## Configuración

Aquí estan los pasos para configurar el entorno de construcción:

1. Asegúrate de contar con los programas de líneas de comando `git`, y `node` version 8 o mayor
2. Clona el repositorio de aplicación (éste): `git clone https://github.com/oskariorg/sample-application.git`
3. Ejecuta `npm install`

## Crea to propia aplicación Oskari

Después de completar la configuración básica (arriba), la aplicación puede construir directamente desde este repositorio con eg. `npm run build`. El output se encontrara debajo de `dist/`.

Mira el principal [repositorio de oskari-frontend](https://github.com/oskariorg/oskari-frontend#readme) para instrucciones detalladas acerca de los parámetros construidos.

### Composición de la aplicación

Una aplicación de Oskari frontend consiste de bundles (módulos) que se definen en la `main.js` para cada aplicación (un ejemplo se encuentra en [aquí](../master/applications/geoportal/main.js)). Sólo los bundles (módulos) referenciados aquí pueden ser ejemplificados al momento de ejecutar.

Las aplicaciones usan el paquete de framework de oskari-frontend para crear estructuras. El framework introduce cargadores que puede ser usados para importar bundles (módulos) de Oskari.

* cargador oskari

   Los bundles (módulos) serán incluidos al bundle js principal de la aplicación.
* cargador-lazy oskari

   Los bundles (módulos) son cargados dinámicamente al momento de ejecutar. Estos bundles no serán incluidos al bundle js principal de la aplicación.

Los bundles (módulos) son utilizados en una parte limitada de la aplicación (por ejemplo herramientas de administrador) pueden ser configuradas para cargar dinámicamente al usar el `oskari-lazy-loader`.
>Usar el cargador lazy disminuirá el tamaño del bundle JS (módulo) principal de la aplicación y acelarará la carga de la página.

### Gestión de dependencias

La aplicación tiene el repositorio de framework de `oskari-frontend` como una dependencia. Esto signifca que puedes referenciar bundles (módulos) en el repositorio `oskari-frontend` con eg. `import 'oskari-loader!oskari-frontend/packages/statistics/statsgrid/bundle.js'` en el main.js.

Si deseas construir un bundle (módulo) personalizados usando componentes oskari-frontend, puedes referenciarlos usando `oskari-frontend` en bundle.js.
```javascript
scripts: [
    {
        type: "text/javascript",
        src: "oskari-frontend/bundles/mapping/mapmodule/AbstractMapModule.js"
    }
]
```

#### Modo de desarrollo de Oskari

El modo de desarrollo de Oskari es útil cuando se desarrolla el framework de Oskari. Porque el framework no contiene una aplicación en sí mismo, la muestra de aplicación puede ser utilizada cuando se desarrollan nuevos carácteristicas de Oskari.

1. Clona el repositorio `oskari-frontend` cercano al repositorio de la aplicación: `git clone https://github.com/oskariorg/oskari-frontend.git`
2. Ejecuta `npm run dev-mode:enable` en el repositorio de la aplicación.

En este modelo, queda a decisición del desarrollador la revisión de las ramas/versiones correctas de la dependencia de `oskari-frontend`.

#### Contribución Oskari frontend

El repositorio `oskari-frontend-contrib` contiene bundles (módulos) no oficiales y aplicaciones para Oskari creadas por la comunidad Oskari. 
Si deseas usar estos bundles, deberías clonar el repositorio de contribución cercano al repositorio de tu aplicación y seguir los siguientes pasos:

1. Clona el repositorio de contribuciones: `git clone https://github.com/oskariorg/oskari-frontend-contrib.git`
2. Accede al directorio de `oskari-frontend-contrib` y ejecuta `npm install`.
3. Edita tu `package.json` de la aplicación, al agregar `"oskari-frontend-contrib": "file:../oskari-frontend-contrib"` para la sección de dependencias.
4. Ejecuta `npm install` en tu directorio de aplicación.

Puedes referencias componentes de las contribuciones usando `oskari-frontend-contrib` de la misma manera en usas `oskari-frontend`.

### Librerías

Antes de agregar una dependencia de librerías (debajo de `libraries/` o vía NPM), debes revisar si la librería esta incluida previamente en el repositorio `oskari-frontend`. Si lo está, puedes referenciarla en tu bundle.js con eg. `oskari-frontend/libraries/geostats/1.5.0/lib/geostats.min.js`. El paquete NPM de dependencias definidas en el repositorio `oskari-frontend` puede ser importado directamente en código fuente en este repositorio eg. Open Layers `import olMap from 'ol/Map';`. Nota: esta no es la forma usual en qué trabaja la resolución del módulo de nodos; esto es una característica especial del sistema de construcción de Oscari cuyo objetivo es evitar el código de duplicación de librería y la versión de conflictos. Para ver qué paquetes pueden ser utilizados de esta forma, mira `dependencies` en [oskari-frontend package.json](https://github.com/oskariorg/oskari-frontend/blob/master/package.json).

Si la librería no está incluida en el repositorio `oskari-frontend`, puedes agregarla ese repositorio, como una dependencia en el paquete.json (preferido) o en `libraries/`. Las dependencias en `libraries/` requieren una referencia en bundle.js, las dependencias en NPM no lo necesitan; sólo `import` con tu código.

### Íconos de aplicación personalizados

Es posible anular cualquier ícono en `oskari-frontend/resources/icons` con íconos específicos para la aplicación. Para agregar íconos para tu aplicación o anularlos: incluye un folder nombrado `icons` debajo del folder la aplicación. Para reemplazar un ícono asigna un archivo png con el mismo nombre como en `oskari-frontend/resources/icons`. Para una compatibilidad máxima, el tamñano de pixeles para el ícono anulado debería igualar el original. Cualquier archivo png en el folder íconos específicos para la aplicación será incluido en el sprite que se genera para ser utilizado para agregar íconos para los bundles (módulos) específicos de la aplicación.

Después de ejecutar la producción de la estructura, es posible crear un conjunto de íconos personalizados para la aplicación al ejecutar el comando `npm run sprite -- [version]:[application path]` como 

    npm run sprite -- 1.0.0:applications

¡Nota! Requiere (GraphicsMagick)[http://www.graphicsmagick.org/] de ser instalado en un servidor y el comando "gm" debe ser utilizable en el cmd/bash.\
¡Note! Primero debes ejecutar una producción de la estructura para la aplicación para crear el dist-folder correspondiente. Con el comando de ejemplo el sprite se generará debajo del folder `dist\1.0.0\geoportal` como `icons.png` y `icons.css`.\
¡Note! Para utilizar los íconos personalizados configura tu HTML (JSP) a las necesidades del servidor de oskari para vincular los íconos.css bajo el folder de la aplicación (Los JSP predefinidos lo vincula bajo oskari-frontend/resources/icons.css).

## Desarrollo del servidor

Ejecuta `npm start` para desarrollar el servidor con auto reload para JS y hot reload para SCSS.

# Reporte de problemas
Todos los problemas relacionados con Oskari deberían ser reportados aquí: https://github.com/oskariorg/oskari-docs/issues

### Preguntas frecuentes

#### Error "Sin memoria" cuando se ejecuta el Webpack

Si recibes un mensaje de error al ejecutar la construcción como "FATAL ERROR: Committing semi space failed. Allocation failed - process out of memory" o "FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - JavaScript heap out of memory" necesitas configurar un poco más de memoria para el proceso nodal.

En linux puedes usar:

    export NODE_OPTIONS=--max_old_space_size=4096
    npm run build

O en Windows:

    set NODE_OPTIONS=--max_old_space_size=4096 && npm run build

#### "Congelación" de producción de estructuras

El uso del CPU de la computadora muestra que nada ocurre, pero el bash/cmd todavía se ejecuta el comando de construcción de estructuras. Intenta configurar "paralelamente" a falso en la configuración UglifyJsPlugin en webpack.config.js:

    new UglifyJsPlugin({
        sourceMap: true,
        parallel: false
    })

#### Fallas de construcción en un error

```
npm ERR! path [...]
npm ERR! code ENOENT
npm ERR! errno -2
npm ERR! syscall rename
npm ERR! enoent ENOENT: no such file or directory, rename '[...]' -> '[...]'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent 
```

Probablemente se debe a la presencia del `package-lock.json` en tu ambiente. El paquete de mecanismos de bloqueo no trabaja adecuadamente con los symlinked node_modules (`oskari-frontend / oskari-frontend-contrib`). Retira el `package-lock.json` por ahora.

## Licencia
 
Este trabajo es una licencia dual bajo MIT y [EUPL v1.1](https://joinup.ec.europa.eu/software/page/eupl/licence-eupl) 
(cualquier versión de lenguaje aplica, la versión en Inglés se incluye en https://github.com/oskariorg/oskari-docs/blob/master/documents/LICENSE-EUPL.pdf).
Puedes elegir entre cualquiera de ellas si lo requires.
 
`SPDX-License-Identifier: MIT OR EUPL-1.1`

Copyright (c) 2014-present National Land Survey of Finland