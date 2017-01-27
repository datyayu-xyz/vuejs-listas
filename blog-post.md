##Intro
- Intro
- disclaimer series
- repo

## Condicionales
Muchas veces ocurre que queremos mostrar cierto contenido solo si se cumple una condicion. La manera en que podemos lograr esto es con `v-if`.

```html
<h1 v-if="mostrar"> Hola <h1>
```

En este caso, Vue solo renderizara este `h1` cuando `mostrar` sea `true`.

Ademas, Vue nos ofrece una manera bastante sencilla de manejar casos en el que queremos mostrar solo uno de varios contenidos usando `v-else` y `v-else-if`.

```html
<h1 v-if="valor === 1"> 1 <h1>
<h1 v-else-if="valor === 2"> 2 <h1>
<h1 v-else-if="valor === 3"> 3 <h1>
<h1 v-else> No 1, 2 o 3 :( <h1>
```

Es bastante sencillo de entender, estas directivas funcionan como cualquier `if/else` normal en javascript.


## v-show
`v-if` es bueno en el sentido de que no renderizara el contenido al cargar la pagina si la propiedad es `false` al inicio. Sin embargo, si ese `v-if` tiende a cambiar mucho durante el transcurso de tu aplicacion, lo mejor es usar `v-show`.

```html
<h1 v-show="mostrar"> Hola <h1>
```

`v-show` es igual a `v-if` en el sentido de que ambos ocultan o muestran el contenido basado en la condicion que les pasemos. Sin embargo, `v-if` remueve por completo el elemento del DOM mientras que `v-show` solo le aplica un `display: none`.

Por esto, utiliza `v-show` si el elemento va a cambiar constantemente y `v-if` si lo mas seguro es que no cambie durante el transcurso de la aplicacion.


## Uso de <template>'s
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

Esto no parece tan malo, pero agregar un `div` puede que llegue a afectar tus estilos de css.

Para evitar esto, podemos usar la etiqueta `<template>` en lugar de `div`.

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
La ventaja aqui es que la etiqueta de `<template>` no se renderiza, solo su contenido, por lo que obtenemos el mismo resultado que con `div` pero sin tener que envolver nuestro contenido en un contenedor extra.


## Iterando un array
- TODO
- modificando un array

## Iterando un objeto
- TODO



## Conclusion
- comentario

Como siempre, los ejemplos de este post están disponible en github para cualquier duda que tengas o mejora que quieras agregar, así que no dudes en hacerlo!

---

*Este post es parte de [una serie acerca de VueJS](/tag/vue-js/). Si tienes dudas o hay alguna funcionalidad que aun no haya cubierto, no dudes en avisarme.*