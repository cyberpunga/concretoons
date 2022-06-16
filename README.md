Este es un trabajo de rescate de _**concretoons: poesía digital**_ para la internet después de Flash.

Puedes encontrar la <a href="https://cyberpun.ga/concretoons/indice.html" target="_blank"><strong>demo aquí</strong></a> y el <a href="https://github.com/cyberpunga/concretoons" target="_blank">código aquí</a>.

![_el laberinto_ de _concretooons_](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zo8k7mzchky42gh765kk.png)

## Introducción

Antes que los _estándares abiertos_ como [HTML5](https://es.wikipedia.org/wiki/HTML5) (2014), [WebGL](https://es.wikipedia.org/wiki/WebGL) o [WebAssembly](https://es.wikipedia.org/wiki/WebAssembly) (ambos lanzados en 2017) alcanzaran su popularidad actual o siquiera existieran, [Adobe Flash Player](https://es.wikipedia.org/wiki/Adobe_Flash_Player) era la opción _por defecto_ de los creadores para la [experimentación artística en la web](https://www.latimes.com/entertainment-arts/story/2020-12-28/end-of-adobe-flash-animation-means-end-of-many-digital-art-projects-internet).

Un ejemplo de esto son los [concretoons](http://concretoons.centroculturadigital.mx/) (2010) de Benjamín Moreno: _formado por 11 poemas digitales, concretoons rinde tributo a algunos de los trabajos más representativos de la poesía experimental iberoamericana_.... [Utilizando] _los videojuegos como medio de creación poética_.

Como parte de su trabajo de documentación, rescate y preservación de la literatura digital, el [Centro de Cultura Digital México](https://www.centroculturadigital.mx/) mantiene _en línea_ esta pieza, donde se puede visitar hasta el día de hoy.

Sin embargo, en 2021 los _concretoons_ y todo el contenido Flash producido durante dos décadas de vida útil del _plugin_ han sido reemplazados por el mensaje de error: _este complemento no es compatible_, despues de que Adobe pusiera [fin al soporte para Flash Player](https://www.adobe.com/es/products/flashplayer/end-of-life.html) el 31 de diciembre de 2020.

![Este complemento no es compatible](https://i.ibb.co/v3F7Prk/error.png)

## Al rescate del contenido Flash perdido

Para comenzar nuestra operación de rescate necesitamos una copia del sitio.

### Descargando el sitio

Para descargar los archivos del sitio utilizamos el comando `wget`, así que abrimos una terminal y ejecutamos lo siguiente.

```sh
$ wget --recursive http://concretoons.centroculturadigital.mx/bbox.html
```

La opción `--recursive` es para decirle a `wget` que descargue el documento `.html` especificado junto con todos sus archivos enlazados, incluyendo otros documentos `.html` junto con todos sus archivos enlazados también.

Al finalizar la descarga tendremos una carpeta llamada `concretoons.centroculturadigital.mx` con la siguiente estructura:

```sh
concretoons.centroculturadigital.mx
├── complementos
│   ├── adelante.jpg
│   ├── atras.jpg
│   ├── casa.jpg
│   ├── concretoon21.swf
│   ├── concretoon22.swf
│   ├── concretoon23a.swf
│   ├── concretoon24.swf
│   ├── concretoon25.swf
│   ├── concretoon26.swf
│   ├── concretoon27.swf
│   ├── concretoon2.swf
│   ├── concretoon34.swf
│   ├── concretoon40.swf
│   ├── concretoon42.swf
│   ├── falso.jpg
│   ├── indice.swf
│   └── info.jpg
├── aqui.html
├── bbox.html
├── borges.html
├── brossa.html
├── carrion.html
├── colofon.html
├── indice.html
├── mallarme.html
├── noigandres.html
├── nokia.html
├── paz.html
└── valium.html

1 carpeta, 29 archivos
```

En la carpeta principal están todos los archivos `.html`, y en la subcarpeta `complementos` están las imágenes de navegación, el `indice.swf` y los 11 archivos `.swf` que componen la colección de _concretoons_.

![Carpeta `complementos`](https://i.ibb.co/BZj4MJf/complementos.png)

Perfecto, ya tenemos una copia del sitio.

### [Ruffle](https://ruffle.rs) y [JPEXS Free Flash Decompiler](https://github.com/jindrapetrik/jpexs-decompiler)

Una búsqueda en GitHub por posibles soluciones para visualizar y editar contenido Flash nos arroja un par de proyectos interesantes:

- [Ruffle](https://ruffle.rs) es un emulador de Flash Player escrito en el lenguaje Rust, y uno de sus _sabores_ puede ejecutarse en el navegador.

- [JPEXS Free Flash Decompiler](https://github.com/jindrapetrik/jpexs-decompiler) es un _decompilador_ y editor de archivos `.swf` escrito en Java y está disponible para Windows, Linux y MacOS.

### Cargando [Ruffle](https://ruffle.rs)

[Ruffle](https://ruffle.rs) existe en 3 _sabores_:

- Aplicación de escritorio
- Extensión para el navegador
- Librería javascript

Esta última (incluye una copia de [Ruffle](https://ruffle.rs) como módulo `.wasm`), tambien llamada _**self hosted**_ (_auto-alojable_), se puede incluir en un archivo `.html` que contenga contenido Flash, y permite que los usuarios vean el contenido sin tener que instalar nada por su lado.

Descargamos una copia desde su [sitio web](https://ruffle.rs) y descomprimimos su contenido en una subcarpeta llamada `lib` que debemos crear en la carpeta principal de nuestro proyecto.

Por último debemos agregar la siguiente línea dentro del elemento `<head>` de nuestros archivos `.html`.

```html
<script src="lib/ruffle.js"></script>
```

Podemos agregar manualmente esta línea, archivo por archivo, o podemos ejecutar el siguiente script para agregarla a todos los archivos `.html` del proyecto.

```bash
for i in *.html;
do sed -i 's/<head>/<head>\n<script src=\"lib\/ruffle.js\"><\/script>/' "$i";
done
```

Listo, eso es todo... bueno, casi.

### Editando con [JPEXS Free Flash Decompiler](https://github.com/jindrapetrik/jpexs-decompiler)

Al [visualizar localmente](#Visualizar-localmente) nuestra copia de los _concretoons_ podemos notar que el `indice.swf` sigue enlazando hacia las piezas alojadas en el sitio original (ej: `http://concretoons.centroculturadigital.mx/nokia.html`).

Para editar nuestro `indice.swf` y hacer que los enlaces lleven hacia nuestras piezas recién rescatadas utilizamos [JPEXS Free Flash Decompiler](https://github.com/jindrapetrik/jpexs-decompiler).

En la primera ventana podemos ver el contenido de nuestro `indice.swf`. Una vez aquí desplegamos `scripts`, donde se encuentran definidos todos los botones.

![editar rutas con jpex](https://i.ibb.co/p0tVdZN/jpex.png)

Al desplegar los botones seleccionamos la opción `BUTTONCONDACTION on(release)`, y en la ventana de la derecha encontramos algo como esto:

```
GetUrl "http://concretoons.centroculturadigital.mx/nokia.html" "_self"

```

Ya que en nuestro proyecto todos los archivos `.html` están en la misma carpeta podemos reemplazarlo por algo como esto:

```
GetUrl "nokia.html" "_self"
```

Debemos realizar esto con todas opciones definidas en el `indice.swf` y ahora sí, eso es todo.

## Visualizar localmente

Si intentamos abrir los archivos `.html` dándoles doble click desde la carpeta local, estaremos utilizando el protocolo `file://`. Esto no funciona porque que los navegadores, _por defecto_, bloquean algunas características al usar este protocolo por motivos de seguridad.

![ruffle file:// protocol error](https://i.ibb.co/sCGqPY2/file.png)

Para ver nuestros archivos `.html` utilizando el protocolo `http://` debemos servir nuestros archivos a través de un servidor web.

Si tenemos instalado NodeJS, una solución rápida es instalar el paquete `nws`.

```bash
# Con npm
npm --global install nws

# O si utilizamos yarn
yarn global add nws
```

Una vez instalado, en una terminal nos dirigimos a la carpeta de nuestros _concretoons_ y ejecutamos lo siguiente.

```bash
nws .
```

Para ver nuestros _concretoons_ en nuestro servidor local podemos dirigirnos a `https://localhost:3030/indice.html` en nuestro navegador.
