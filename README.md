# Development Notes


Para Solsteace La mayoria de las aplicaciones estan implementadas con la tecnologia de AngularJS, para desarrollar front-ends de páginas web. Durante dichos desarrollosse quiere mantener prácticas de estandarizacion para organizar el código lo más limpio y sostenible posible de forma que puedan adaptarse al crecimiento. Buscando seguir  en las guías de estilos de @John Papa y @Todd Motto.

A continuación listo las practicas que en todo desarrollo tendremos en cuenta en Solsteace. De igual manera aca estan de forma adjunta las guias de estilos que tomamos en cuenta: [John Papa](https://github.com/johnpapa/angular-styleguide) y guía de estilos de [Todd Motto](https://github.com/toddmotto/angularjs-styleguide).

## Codigo Ravioli vs Codigo Espagueti

Cada archivo del proyecto tiene que tener una única responsabilidad, sin importar el número de líneas que pueda tener. Cada responsabilidad hay que encapsularla y aislarla para conseguir código ravioli, es totalmente posible tener pocos archivos que junten modules, controllers y directives. Pero la idea es tener el codigo lo mas modular posible.

Aunque, en un principio, tengamos la tentación de unificar varias funcionalidades con la excusa de que son pocas líneas, es muy importante empezar a separar el código desde el principio. A medida de que el proyecto vaya creciendo, aumentarán las  líneas de código y si no seguimos esta buena práctica, nos encontraremos con código espagueti. Si evitamos esto asi sea desde proyectos pequeños, le damos la posibilidad de escalar enormemente.

**Debemos evitar:**

```javascript
angular
    .module('app', ['ui.router'])
    .controller('SomeController', SomeController)
    .factory('someFactory', someFactory);

    function SomeController() { }

    function someFactory() { }
```

**Debemos Organizar de la siguiente manera:**


### app.js
```javascript
angular
    .module('app', ['ui.router']);

```

### algunController.js
```javascript
angular
    .module('app')
    .controller('SomeController', SomeController);

    function SomeController() { }

```
### algunaFactory.js
```javascript
angular
    .module('app')
    .factory('someFactory', someFactory);

    function someFactory() { }

```

## IIFE (Immediately Invoked Function Expression)


Es una buena práctica empaquetar los componentes en **funciones que se invocan inmediatamente**. De ese modo, evitamos que las variables y funciones estén más tiempo de lo esperado en el global scope, al mismo tiempo que evitamos posibles colisiones entre variables.

**Debemos evitar:**

```javascript
angular
    .module('app')
    .factory('logger', logger);

    // la función logger es añadida como variable global
    function logger() { }
```

**Debemos Organizar de la siguiente manera:**

```javascript
(function() {
    'use strict';

    angular
        .module('app')
        .factory('logger', logger);

    function logger() {}
})();
```
