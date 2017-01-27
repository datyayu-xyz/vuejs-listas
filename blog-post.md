## Vue.js: Condicionales y listas.

Ya hemos revisado como usar Vue.js para interactuar con el DOM de manera sencilla pero ahora toca ver como mostrar contenido basado en una o varias condiciones y como trabajar con listas de la manera correcta.

*Este post es parte de [una serie acerca de VueJS](/tag/vue-js/). Si eres nuevo usando Vue.js y no has leido los post anteriores, te recomiendo leer la [Introduccion a Vue.js](/vuejs-intro) y [como interactuar con el DOM usando Vue](/vuejs-dom) para que tengas una mejor comprension de este post*

## Condicionales
Muchas veces ocurre que queremos mostrar cierto contenido solo si se cumple una condicion, como el caso de mostrar un avatar si el usuario esta logueado. La manera en que podemos lograr esto es con `v-if`.

```html
<div id="app">
    <boton @click="cambiarEstado">Mostrar/Ocultar</boton>
    <h1 v-if="mostrar"> Hola <h1>
</div>
```
```js
new Vue({
    el: '#app',
    data: {
        mostrar: true
    },
    methods: {
        cambiarEstado: function() {
            this.mostrar = !this.mostrar;
        }
    }
})
```
Aqui tenemos que el `h1` utiliza una directiva de Vue, `v-if`, y lo que hace es que solo renderizara el contenido cuando la variable `mostrar` dentro de la instancia de Vue sea `true`. Tambien tenemos un boton que al ser clickeado ejecuta el metodo `cambiarEstado`, el cual solo invierte el valor de `mostar`, cambiandolo a `false` si era `true` y viceversa.

Ademas, Vue nos ofrece una manera bastante sencilla de manejar casos en los que queremos mostrar solo uno de varios contenidos usando `v-else` y `v-else-if`.

```html
<div id="app">
    <button @click="sumarUno"></button>
    <h1 v-if="numero === 1"> Uno <h1>
    <h1 v-else-if="numero === 2"> Dos <h1>
    <h1 v-else-if="numero === 3"> Tres <h1>
    <h1 v-else> No 1, 2 o 3 :( <h1>
</div>
```
```js
new Vue({
    el: '#app',
    data: {
        numero: 1
    },
    methods: {
        sumarUno: function() {
            this.numero++;
        }
    }
})
```

Es bastante sencillo de entender, estas directivas funcionan como cualquier `if/else` normal en javascript. Al clickear el boton, se cambia el valor de la variable `numero`
y Vue se encarga de mostrar el caso que corresponda al valor actual, o utiliza `v-else` si ninguno de los casos se cumple.

## v-show
`v-if` es bueno en el sentido de que no renderizara el contenido al cargar la pagina si la propiedad es `false` al inicio, mejorando el tiempo de carga inicial. Sin embargo, si ese `v-if` tiende a cambiar mucho durante el transcurso de tu aplicacion, lo mejor es usar `v-show`.

```html
<h1 v-show="mostrar"> Hola <h1>
```

`v-show` es igual a `v-if` en el sentido de que ambos se usan igual y ocultan o muestran el contenido basado en la condicion que les pasemos. Sin embargo, `v-if` remueve por completo el elemento del DOM mientras que `v-show` solo le aplica un `display: none;` al estilo del elemento.

Por esto, la recomendacion es utilizar `v-show` si el elemento va a cambiar constantemente y `v-if` si lo mas seguro es que no cambie durante el transcurso de la aplicacion.


## Iterando un array
Parte fundamental de la mayoria de las aplicaciones es el trabajar con listas de datos. Ya sea un simple array sencillo o algo mas complejo como una lista de objetos, su uso con Vue es bastante sencillo. La manera en que lo hacemos es usando la directiva `v-for`.
```html
<ul id="app">
    <li v-for="nombre in nombres">
        {{ nombre }}
    </li>
</ul>
```
```js
new Vue({
    el: '#app',
    data: {
        nombres: [ 'pepe', 'beto', 'max' ]
    }
})
```
El valor de `v-for` tiene el formato `[elemento] in [lista_de_elementos]`. Lo que hace Vue es iterar la `[lista_de_elementos]` y `[elemento]` representa al elemento actual que va recorriendo. Por cierto, el nombre del `[elemento]` puede ser cualquiera que tu quieras ponerle.


## Iterando con indice
Muchas veces ademas del valor, tambien queremos obtener el indice de la iteracion en la que vamos. Para hacer esto con Vue, solo hace falta usar la sintaxis
```js
<li v-for="(elemento, indice) in lista_de_elementos">
    {{ indice }} - {{ nombre }}
</li>
```

Donde en lugar de guardar solo el valor del elemento, declaramos una tupla (`(elemento, indice)`) en la que la primera posicion (`elemento`) es el valor actual del elemento y la segunda (`indice`) el indice del mismo.


## Iterando una lista de objetos
Iterar sobre una lista de objetos es exactamente lo mismo que hacerlo en un array sencillo.
```html
<ul id="app">
    <li v-for="persona in personas">
        {{ persona.nombre }} - {{ persona.edad }}
    </li>
</ul>
```
```js
new Vue({
    el: '#app',
    data: {
        personas: [
            { nombre: 'pepe', edad: 30 },
            { nombre: 'beto', edad: 25 },
            { nombre: 'max',  edad: 20 },
        ]
    }
})
```

La sintaxis es exactamente la misma, lo unico que cambia es que, como es un objeto, debemos acceder a sus propiedades usando `objeto.propiedad` (como `persona.nombre`), pero fuera de eso funciona exactamente igual que como ya lo hemos visto.


## Como modificar un array.
Cuando modificas un array en los metodos de tu instancia de Vue, debes utilizar los metodos genericos de un array, como `push`, `pop`, `shift`, `unshift`, `splice`, `sort`,y `reverse`. Esto es debido a que Vue tiene optimizaciones enternas que le permiten detectar estos metodos y revisar los cambios para asi modificar el DOM de la manera mas eficiente posible.

Entonces, para agregar un elemento utilizarias algo como:
```js
new Vue({
    //...
    methods: {
        agregarPersona: function() {
            this.personas.push({ nombre: 'jorge', edad: 28 })
        }
    }
})
```
De esta manera Vue internamente reconocera que el array de `personas` cambio y actualizara el DOM para reflejarlo.

Vue tambien reconoce cuando reasignas el valor a un nuevo array, como al usar `filter`, `concat`, `reduce`, `slice` o `map`.
Por ejemplo:
```js
new Vue({
    //...
    methods: {
        filtrarPersonas: function() {
            this.personas = this.personas.filter(function(persona) {
                return persona.edad > 18;
            });
        }
    }
})
```

Contrario a lo que pareceria, a pesar de que estas creando un nuevo array al usar `filter` en lugar de modificar el que ya tenias, Vue.js es inteligente y solo realizara las operaciones minimas requeridas, dejando intactos los elementos que ya estaban presentes originalmente y solo removiendo los que ya no quieres mostrar.

## Como **NO** modificar un array
Debido a las optimizaciones internas que Vue realiza para ser mas rapido y eficiente, hay que tener cuidado pues hay dos casos en los que Vue puede que no detecte tus cambios y genere errores en tu aplicacion. Por ello, las recomendaciones son:

#### 1. **NO modificar accediendo con el indice**.
Por ejemplo:
```js
this.personas[indice] = nuevoValor;
```
En su lugar, puedes remplazarlo por:
```js
this.personas.splice(indice, 1, nuevoValor);
```
Que realiza exactamente lo mismo pero si es registrado por el detector de cambios de Vue.

#### 2. ***NO modificar el tamano del array directamente**.
Por ejemplo:
```js
this.personas.length = nuevoTamano;
```
Como remplazo, puedes usar
```js
this.personas.splice(nuevoTamano);
```

## Uso de &lt;template&gt;'s
Una de las limitaciones al usar `v-if`, `v-show` o `v-for` es que solo se declaran en un elemento, por lo que si queremos mostrar y ocultar un bloque de contenido nos vemos obligados a agregar un `div` que los contenga.

Por ejemplo:
```html
<!-- Login con usuario -->
<label>Usuario</label>
<input type="text" placeholder="Introduce tu usuario">

<!-- Login con correo -->
<label>Email</label>
<input type="email" placeholder="Introduce tu email">
```

En este caso, lo logico seria agruparlos en `div` para poder usar `v-if`.
```html
<div v-if="tipoDeLogin === 'usuario'">
    <label>Usuario</label>
    <input type="text" placeholder="Introduce tu usuario">
</div>
<div v-else>
    <label>Email</label>
    <input type="email" placeholder="Introduce tu email">
</div>
```

Esto no parece tan malo, pero agregar un `div` puede que llegue a romper tus estilos de css.

Para evitar esto, podemos usar la etiqueta `<template>` en lugar de `<div>`.
```html
<template v-if="tipoDeLogin === 'usuario'">
    <label>Usuario</label>
    <input type="text" placeholder="Introduce tu usuario">
</template>
<template v-else>
    <label>Email</label>
    <input type="email" placeholder="Introduce tu email">
</template>
```
La ventaja aqui es que la etiqueta de `<template>` no se renderiza, solo su contenido, por lo que obtenemos el mismo resultado que con `div` pero sin tener que envolver a todos los elementos en un contenedor extra.

---

Con todo este conocimiento ya tienes mas que suficiente para crear aplicaciones usando Vue, felicidades! En el siguiente post crearemos una pequena app para poner a prueba estos conceptos y ver como todo se complementa para hacer algo funcional.

Como siempre, los ejemplos de este post están disponible en github para cualquier duda que tengas o mejora que quieras agregar, así que no dudes en hacerlo!

---

*Este post es parte de [una serie acerca de VueJS](/tag/vue-js/). Si tienes dudas o hay alguna funcionalidad que aun no haya cubierto, no dudes en avisarme.*