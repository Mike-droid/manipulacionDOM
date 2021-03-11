# Curso de manipulación del DOM

## DOM y Web API

### Y entonces nacio internet

[El tutorial de JS moderno](https://es.javascript.info/)

[DOM documentación de Mozilla](https://developer.mozilla.org/es/docs/Web/API/Document_Object_Model/Introduction)

### ¿Qué es el DOM?

Todo empieza en el proceso de Critical Rendering Path, de aquí el navegador crea el DOM (Document Object Model) y el CSSOM (CSS Object Model).
Son árboles (estructuras de datos).

DOM:

- Es una representación del HTML.
- Estructura en forma de árbol de nodos.
- Es un modelo que puede ser modificado.

### Web APIs modernas

DOM + Javascript = Web API

API es un puente entre el DOM y JS para que podamos manipular el DOM usando JS.

Existen más de 70 APIs, el DOM es solo 1 de ellas.

**Biblias del desarrollo web:**

- ¿Cómo lo uso? -> [Developer mozilla](https://developer.mozilla.org)
- ¿Puedo usarlo? -> [Can I use](https://caniuse.com)

## Operaciones básicas

### Leer nodos

```javascript
document.getElementById('id');
```

```javascript
document.getElementsByTagName('tagName');
```

```javascript
document.getElementsByClassName('className');
```

No es muy conveniente usar estos selectores cuando usamos apps reales.

```javascript
document.querySelector('selector'); //Está muy ligado con los selectores de CSS
```

```javascript
document.querySelectorAll('selector');
```

Pero estos 2 son mucho mejores, nos darán la mayor flexibilidad y beneficio cuando estemos manipulando el DOM.

### NodeLists vs Array

La principal diferencia entre entre ambos es que Nodelist carece de los métodos de array.

Podemos convertir un nodelist a un array muy fácilmente:

```javascript
const nodeList = document.querySelectorAll('div'); //Regresa 50 elementos, por ejemplo
const nodeListAsArray = [...nodeList] //Con esto ya convertimos la lista de nodos en un array :D
```

**Protip:** SIEMPRE que puedas, usa arrays en vez de nodelists, los navegadores están mejor optimizados para esta estructura de datos.

### Crear y agregar

```javascript
document.createElement(elem); //pasamos una etiqueta entre comillas
/*Creamos un elemento en memoria pero NO se agrega al DOM*/

document.createTextNode(data); //Esto simplemente es texto
```

Hay muchas formas de agregas nodos al DOM, depende de las necesidades, algunos métodos son:

```javascript
node.appendChild(element);
parentNode.append(Nodes/DOMStrings); //append es la evolución de append child, puedes agregar más de 1 nodo, puedes agregar texto
node.insertBefore(newNode, referenceNode); // El nodo de referencia tiene que ser hijo directo del nodo base
element.insertAdjacentElement(posicion,element); // posisción puede ser: 'beforebegin'|'afterbegin'|'beforeend'|'afterend'
```

### Otras formas de agregar

**Protip:** usando `$0` podemos acceder al elemento HTMl que esté seleccionado en el inspector.

```javascript
htmlElement.outerHTML = 'DOMString'; //Leer
htmlElement.innerHTML = 'DOMString'; //Escribir
```

innerHTML tiene problemas de inyecciones XSS, hay que tener cuidado con esto.

### Atributos y propiedades

Los **atributos y propiedades** son los que realmente le dan vida a los elementos que se encuentran en el DOM. El 80% del tiempo que estemos manipulando el DOM, realmente lo que nosotros estamos haciendo, es cambiar de forma dinámica con JavaScript las propiedades de un elemento para adecuarlo a nuestras necesidades.

***PREGUNTA DE ENTREVISTA: ¿CUÁL ES LA DIFERENCIA ENTRE UN ATRIBUTO Y UNA PROPIEDAD?*** : Los atributos son usados únicamente al inicio del HTML.
Las propiedades pueden cambiar en cualquier momento, pero los atributos son usados únicamente al principio para inicializar el HTML.
En el DOM vemos los atributos y en la página web veremos las propiedades.

```javascript
// Al seleccionar el nodo HTML, JavaScript lo convierte en un objeto!
const input = document.querySelector("input")

// Y of course, podemos modificarlo como cualquier otro objeto de JavaScript:
input.placeholder = "Escribe algo"
input.value = 2
input.type = "number"
```

### Eliminar nodos

```javascript
node.removeChild(child);
childNode.remove(); //Es la evolución de removeChild
parentNode.replaceChild(newChild, oldChild);
```

Podemos acceder al padre de un elemento usando:

```javascript
const parentElement = node.parentElement;
```

### Operaciones en lote

Hacer operaciones en el DOM es algo muy costoso. Entre menos operaciones hagamos en el DOM *especialmente escribir y eliminar* , más rápida será nuestra operación.

Ejemplo, agregar 100 inputs al final de body.

Forma NO óptima: (modificamos el DOM 100 veces)

```javascript
for (let i = 0; i<100; i++){
    const node = document.createElement('input')
    document.body.appendChild(node)
}
```

Mejor forma: (modificamos el DOM 1 vez)

```javascript
const nodos = []

for (let i = 0; i<100; i++){
    const node = document.createElement('input')
    nodos.push(node)
}

document.body.append(...nodos)
```

[Documentación de operador de propagación](https://developer.mozilla.org/es/docs/conflicting/Web/JavaScript/Reference/Operators/Spread_syntax)

## Workshop 1: Fetch

[Documentación de Fetch API en Mozilla](https://developer.mozilla.org/es/docs/Web/API/Fetch_API)

### Descargando información y creando nodos

[Repositorio del primer proyecto](https://github.com/jonalvarezz/snowpack-template-tailwind)

`npx create-snowpack-app my-app --template snowpack-template-tailwind`

[Link de la API de avocados](https://platzi-avo.vercel.app/api/avo)

### Enriqueciendo la información

Los elementos de texto pueden ser agregados a la página web con `textContent` y las imágenes con `src`.

Es una práctica común (y profesional) crear un contenedor con un id para que JS lo use para renderizar el contenido de una API.

### Usando la API de internacionalización del browser

Podemos agregarle clases CSS al rederizado desde JS y en CSS modificar estos estilos, por ejemplo:

```javascript
    //title.style = 'font-size: 2rem'; // modificamos los estilos desde JS
    //title.style.fontSize = "3rem";
    title.className = 'muy-grande'; //* Agregamos la clase al nodo
```

```css
.muy-grande{
  font-size: 3rem;
  color: blue;
}
```

En este proyecto en particular estamos usando Tailwind, un framework de CSS, podemos agregar la clase de sus componentes y estos estilos también serán aplicados al proyecto:

```javascript
title.className = 'text-2xl text-red-600';
```

No olvides que existe `classList.add(className)` y `classList.remove(className)` para poder quitar y agregar clases de CSS a las etiquetas.

INTL es una API para dar formato a monedas y fechas.

***¡Recuerda!***: no es necesario que te aprendas ninguna API de memoria, solo debes saber que existen, saber para qué se usan y apoyarte de la documentación para sacarle el máximo provecho.

## Eventos

### Reaccionar a lo que sucede en el DOM

JS es un lenguaje que está basado en eventos.

```javascript
document.addEventListener('type', listener); //Agregamos un evento
document.removeEventListener('type', listener); //Eliminamos un evento
```

Ejemplos sencillo y comunes:

```javascript
const accion = () => {
    console.log('Has hecho click en el input');
}

const input = $0 //Ya tenemos un elemento guardado aquí

input.addEventListener('click', accion);

input.addEventListener('input', ()=>{
    console.log('hey'); //Cada vez que insertermos o borremos algo en el input y/o text area, se ejecutará esto
});
```

Absolutamente todos los eventos nos envían un parámetro "event" y nos da mucha información.

```javascript
input.addEventListener('input' , event => {
    console.log(event);
});

input.addEventListener('input' , event => {
    if(event.data === 'p'){
        console.log('Escribiste una p')
    }
});
```

```javascript
const email = $0

const accion1 = () => console.log('acción1')
const accion2 = () => console.log('acción2')

email.addEventListener('click', accion1);
email.addEventListener('click', accion2);
email.removeEventListener('click', accion1);
```

No es conveniente usar funciones anónimas si las vamos a borrar porque al ser anónimas, no podrán ser enviadas como parámetros en `removeEventListener`

[Documentación oficial de Referencia de eventos en JS](https://developer.mozilla.org/es/docs/Web/Events)

### Event propagation

Siempre toma en cuenta que los eventos se propagan desde el elemento más interno hacía afuera. A esto se le llama Bubble.

```javascript
const accion = event => console.log(`Hola desde: ${event.currentTarget.nodeName}`)

$0.addEventListener('click',accion) //titulo
$0.addEventListener('click',accion) //div
$0.addEventListener('click',accion) //body

/*
Resultado is hacemos click en el titulo:
Hola desde: H2
Hola desde: DIV
Hola desde: BODY

Resultado is hacemos click en el div:
Hola desde: DIV
Hola desde: BODY

Resultado is hacemos click en el body:
Hola desde: BODY
*/
```

También podemos evitar la propagación:

```javascript
$0.addEventListener('click', event => {
  event.stopPropagation();
  console.log(`Hola desde: ${event.currentTarget.nodeName}`);
});
```

Pero por lo general no usaremos esto y deberiamos dejar que el DOM funcione como ya lo hace.

### Event delegation

Usar la delegación de eventos nos ayudará mucho para evitar tener que poner eventos por cada elemento. Esto lo debemos usar siempre que podamos cuando tengamos muchos listeners.

```javascript
const appNode = document.querySelector('#app');

appNode.addEventListener('click', event => {
  if (event.target.nodeName === 'H2') { //Si el elemento es un H2 haz esto, y funciona para todos los H2
    window.alert('hola');
  }
});
```

## Workshop 2: Lazy loading

### Presentación del proyecto

[Repositorio del proyecto](https://github.com/jonalvarezz/platzi-dom/tree/main/workshop-lazy-loading)

[Link de las imágenes de los zorros(primera foto)](https://randomfox.ca/images/1.jpg)

### Creando las imagenes con JavaScript

### Intersection Observer

Nos permite saber qué parte de la pantalla estamos viendo con el Viewport.

[Documentación oficial de Intersection Observer API](https://developer.mozilla.org/es/docs/Web/API/Intersection_Observer_API)

### Aplicando Lazy loading

dataset se utiliza mucho para comunicar información entre HTML y JS (en el browser aparece como data-src).

## Workshop 3

### Proyectos propuestos

[HTML Audio and Video DOM Reference](https://www.w3schools.com/tags/ref_av_dom.asp)

[API de Open Weather](https://openweathermap.org/api)

## Librerías relacionadas

### JQuery

Fue muy popular en años anteriores porque no era fácil manipular el DOM. Hoy en día no es necesario en la gran mayoría de los proyectos. Puede servir en los navegadores antiguos.

### JSX

Es lo que hace parecer que en React escribamos HTML en JS. JSX es muy similar a hyperscript.
