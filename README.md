# proyecto-recetas
---
title: "PEC1 Herramientas en HTML y CSS I"
author: "Pilar Lopez Fernandez"
date: "15/3/2025"
output: 
  html_document:
    theme: united
    highlight: zenburn
    toc: true
    toc_float: true
---

```{r setup, include=TRUE, eval=FALSE}
knitr::opts_chunk$set(echo = FALSE)
```
En este módulo has aprendido qué es y cómo funciona un entorno de desarrollo moderno y dispones de herramientas que te ayudan a desarrollar sitios web. ¡Esta PEC es el momento de demostrar y poner en práctica todos los conocimientos adquiridos!

Os proponemos el reto de desarrollar un sitio web. Las Actividades que os proponemos realizar en el Módulo 2 os resultarán muy útiles. La práctica tiene unos requisitos mínimos que se especifican en el apartado de Descripción. Todo el proceso que sigas deberá estar documentado, y es necesario que entregues exactamente lo que se pide en el apartado de Formato y fecha de entrega para que la práctica pueda ser evaluada.

## Objetivos {.tabset .tabset-pills}
#### 1. Diseñar y ejecutar un pequeño sitio web responsive
Esto significa crear un sitio web que se adapte automáticamente a diferentes tamaños de pantalla (móviles, tablets, ordenadores). Un sitio responsive:

  - Reorganiza elementos según el tamaño de pantalla
  
  - Ajusta imágenes y contenido
  
  - Tiene menús adaptables
  
  - Utiliza unidades relativas (%, em, rem, vh/vw) en lugar de píxeles fijos
  
  - Implementa media queries (@media) para definir estilos según el ancho de pantalla

#### 2. Utilizar el inspector DOM, el editor de estilos y las herramientas para diseño responsive del navegador como ayuda para el desarrollo
Debes usar las herramientas de desarrollo del navegador para:

  - Inspeccionar y modificar el DOM (estructura HTML) en tiempo real
  
  - Probar y ajustar estilos CSS directamente en el navegador
  
  - Simular diferentes dispositivos (móvil, tablet) con el modo responsive
  
  - Depurar problemas de diseño y rendimiento
  
  - Ver cómo se comporta tu sitio en distintas resoluciones

#### 3. Utilizar un workflow de desarrollo frontend moderno, que diferencie entornos de desarrollo y producción
Debes implementar un flujo de trabajo que separe:

  - *Entorno de desarrollo*: optimizado para programadores, con código legible, comentado, sin minificar, con herramientas de desarrollo activadas (hot reload, sourcemaps, etc.)

  - *Entorno de producción*: optimizado para usuarios finales, con código minificado, comprimido, sin comentarios innecesarios, con assets optimizados

Este workflow generalmente usa herramientas como Webpack, Vite, Gulp o similar para automatizar tareas.

#### 4. Crear una web utilizando un boilerplate que permita escribir código moderno y que sea transformado para hacerlo ejecutable en navegadores antiguos
Debes usar una plantilla de inicio (boilerplate) que:

  - Te permita usar JavaScript moderno (ES6+)

  - Incluya transpiladores como Babel para convertir código moderno a versiones compatibles con navegadores antiguos

  - Configure herramientas como PostCSS o autoprefixer para CSS

  - Gestione la compatibilidad con versiones antiguas (polyfills)
Algunos ejemplos: Create React App, Vue CLI, HTML5 Boilerplate, etc.

#### 5. Publicar la web
Debes subir tu sitio web a internet para que sea accesible públicamente:

  - Elegir un servicio de hosting (GitHub Pages, Netlify, Vercel, etc.)

  - Configurar correctamente los archivos de producción

  - Ajustar configuraciones de dominio si es necesario

  - Verificar que todo funcione correctamente una vez publicado

  - Comprobar rendimiento y accesibilidad en la versión publicada

## 1. Configuración inicial del proyecto
### 1.1 Inicializar Git y GitHUb
Creamos la carpeta principal del proyecto.
```{bash, include=TRUE, eval=FALSE}
## Se crea la carpeta del proyecto
mkdir proyecto-recetas
cd proyecto-recetas
```

Mediante los comandos en bash inicializamos GIT:
```{bash, include=TRUE, eval=FALSE}
## Se inicializa repositorio Git .git
git init
git add README.md
git commit -m "primer commit"
git branch -M main
git remote add origin https://github.com/PilarLF/Web-HTML_CSS_I.git
git push -u origin main
(falta contraseña para que esto último funcione)
```

#### 1.1.1 Configuración del archivo .gitignore.
Este archivo es una herramienta importante para la gestión de proyectos con Git. Este fichero le indica a Git qué archivos o carpetas debe ignorar y no incluir en el control de versiones. De este modo se evita subir archivos innecesarios, reducir el tamaño del repositorio y prevenir conflictos. Para crear este archivo puede utilizarse un editor de texto (p.e. VScode) o mediante línea de comandos:
```{bash, eval=FALSE}
touch .gitignore
### dependencias
echo "node_modules/" > .gitignore

### archivos de cache y construccion
echo ".parcel-cache/" >> .gitignore
echo "dist/">> .gitignore
echo "build/">> .gitignore

### archivos de entorno
echo ".env" >> .gitignore
echo ".env.local" >> .gitignore

### archivos del sistema
echo ".DS_Store" >> .gitignore
echo "Thumbs.db" >> .gitignore

# Archivos de registro
echo "npm-debug.log*" >> .gitignore
echo "yarn-debug.log*" >> .gitignore
echo "yarn-error.log*" >> .gitignore
echo "*.log" >> .gitignore

# Archivos de editor
echo ".idea/" >> .gitignore
echo ".vscode/" >> .gitignore
echo "*.swp" >> .gitignore
echo "*.swo" >> .gitignore
```

![Creación del fichero .gitignore con las especificaciones necesarias.](/home/pilofer/Imágenes/captura.png){width=width height=height}  
```{bash, eval=FALSE} 
### Configurar el archivo .gitignore
node_modules/
.parcel-cache/
dist/
.DS_Store
*.log
```

Para verificar que .gitignore funciona:
```{bash, eval=FALSE}
git status
### no debería aparecer listados los ficheros y carpetas especificados anteriormente
```

## 2. Crear la plantilla de inicio con Parcel (Boilerplate)
Antes de empezar hay que tener instalado Node o npm y tener creado el directorio del proyecto. 
Parcel es un module bundler, es decir, permite automatizar una serie de tareas así como gestionar las dependencias que hemos instalado con npm. Webpack es otro ejemplo de module bundler que ya hemos visto. 
Parcel es un empaquetador de aplicaciones web (bundler). Destaca poor su simplicidad y configuración "zero-config". A diferencia de otros bundlers como Webpack, no requiere configuraciones extensas. Características principales de Parcel:
1. Configuración automática: no necesita un archivo de configuración para funcionar. 
2. Rápido: utiliza procesamiento paralelo y caché para optimizar el rendimiento. 
3. Soporte para múltiples lenguajes (JavaScript, TYpeScript, SCSS, CSS, HTML... )
4. Hot Module Replacement (HMR). Actualiza los módulos del navegador sin necesidad de recargar la página. 
5. Code splitting. Divide automáticamente el código en diferentes archivos para optimizar la carga. 

### 2.1 Inicializamos NPM.
```{bash, eval=FALSE}
#Primero inicializamos NPM
npm init -y
```
![Inicialización de npm](/home/pilofer/Imágenes/npm.png){width=width height=height} 

### 2.2 Instalamos Parcel y otras dependencias básicas

```{bash,  eval=FALSE}
## Instalamos el bundle y las dependencias básicas
npm install --save-dev parcel


# Procesador SASS para CSS
npm install --save-dev sass

# Babel para compatibilidad JavaScript
npm install --save-dev @babel/core @babel/preset-env

# Una librería para efectos de imagen (como ejemplo de dependencia externa)
npm install aos

# Dependencia para optimización de imágenes
npm install --save-dev sharp

npm install --save-dev postcss autoprefixer
```

Podemos comprobar que hemos instalado **SASS como preprocesador CSS** y **BABEL cpara la compatibilidad JavaScript**. Babel es un **transpilador**, y permite convertir código moderno a versiones compatibles con otros navegadores. **PostCSS** y **autoprefixer** son herramientas para CSS. AOS (Animate On Scroll) será útil para añadir animaciones al desplazarse por la página. 

![Instalación de Parcel](/home/pilofer/Imágenes/imagen.png){width=width height=height} 

### 2.3 Configurar scripts en package.json
Podemos ver que se ha creado el fichero package.json al inicializar npm. Abrimos el archivo y modificamos la sección "scripts":
```{bash, eval=FALSE}
"scripts": {
  "start": "parcel src/*.html",
  "build": "parcel build src/*.html"
}
```
*package.json* es un componente central en proyectos de JavaScript y Node.js. Define comandos personalizados que pueden ser ejecutados mediante npm run [nombre-script] y gestión de dependencias, ya que lista todas las librerías y paquetes que el proyecto necesita. Cuando alguien ejecuta npm install, npm lee este archivo y descarga las dependencias especificadas. 

![Archivo **packaje.json**](/home/pilofer/Imágenes/package.png){width=width height=height} 

### 2.4 Crear la estructura básica de archivos
```{bash, eval=FALSE}
mkdir -p src/assets/images
mkdir -p src/assets/videos
mkdir -p src/styles/scss
mkdir -p src/scripts
```
Se puede observar que estamos usando scss (Sassy CSS) y no CSS, ya que es un preprocesador de sintaxis de Sass. Con él podemos mejorar la mantenibilidad y legibilidad del código. Luego, herramientas como Parcel compilan automáticamente estos archivos SCSS a CSS normal que el navegador puede entender. 

## 3. Crear archivo HTML 

### 3.1 Portada (src/index.html)

## 4. Desarrollo de CSS con SASS

### 4.1 styles.scss

#### src/styles/scss/main.scss

## 5. Crear archivo JavaScript (src/scripts/main.js)

## 6. Crear páginas adicionales. 
#### src/categorias.html
#### src/enlaces.html
#### src/detalle1.html
#### src/detalle2.html
