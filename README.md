# Development Notes


Para Solsteace La mayoria de las aplicaciones estan implementadas con la tecnologia de AngularJS, para el desarrollo front-end. Durante dichos desarrollos se quiere mantener prácticas de estandarizacion para organizar el código lo más limpio y sostenible posible de forma que puedan adaptarse al crecimiento. Buscando seguir las guías de estilos de @John Papa y @Todd Motto.

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


##### app.js
```javascript
angular
    .module('app', ['ui.router']);

```

##### algunController.js
```javascript
angular
    .module('app')
    .controller('SomeController', SomeController);

    function SomeController() { }

```
##### algunaFactory.js
```javascript
angular
    .module('app')
    .factory('someFactory', someFactory);

    function someFactory() { }

```

##### IIFE (Immediately Invoked Function Expression)


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



## Evitar colisiones de nombres


Para evitar la colisión de nombres, utilizaremos una convención de nombres única con separadores para los sub-módulos. Además el uso de separadores ayuda a definir los módulos y sus jerarquías.

Por ejemplo, app puede ser nuestro módulo raíz y app.dashboard y app.users pueden ser módulos que dependen de él.

Declararemos los módulos sin asignarlos a una variable y lo haremos utilizando la sintaxis de los setters, de forma que usaremos angular.module(‘app’, []) para declarar un módulo y especificar sus dependencias (sintaxis setter) y angular.module(‘app’) para recuperar un módulo (sintaxis getter).

**Debemos evitar:**

```javascript
var app = angular.module('app', [
    'ngAnimate',
    'ui.router',
    'app.shared'
]);
```


**Debemos Organizar de la siguiente manera:**

```javascript
angular
    .module('app', [
        'ngAnimate',
        'ui.router',
        'app.shared'
    ]);

```

## Controller as vm

Utilizaremos la sintaxis controller as en vez del clásico controller con $scope y capturaremos el this en una variable llamada vm (ViewModel).

Este método nos ayuda a prevenir el uso excesivo del $scope, ya que en algunos casos suele ser muy tentador utilizar dicha variable para declarar funciones que deberían de estar delegados a un servicio o factoría. El $scope se debe utilizar únicamente cuando es necesario; por ejemplo, al llamar a eventos utilizando $emit, $broadcast o $on.


**Debemos evitar:**
```javascript
function Customer() {

    this.name = 'Irontec';

    this.sendMessage = function() {};
}

```
**Debemos Organizar de la siguiente manera:**
```javascript
function Customer() {

    var vm = this;

    vm.name = 'Irontec';

    vm.sendMessage = function() {};
}
```


## Funciones anonimas vs Funciones declaradas

En la medida de lo posible utilizaremos funciones declaradas, ya que el código será más limpio, más fácil de debuggear y reducirá la cantidad de llamadas anidadas.

**Debemos evitar:**
```javascript
angular
    .module('app')
    .controller('DashboardController', function() { })
```
**Debemos Organizar de la siguiente manera:**
```javascript

angular
    .module('app')
    .controller('DashboardController', DashboardController);

function DashboardController() { }

```

Tambien se refleja el ejemplo para un Factory.

**Debemos evitar:**
```javascript
angular
    .module('app')
    .factory('logger', function() { });

```
**Debemos Organizar de la siguiente manera:**
```javascript

angular
    .module('app')
    .factory('logger', logger);

function logger() { }

```



## Asociaciones en la parte superior del controlador


Colocaremos las asociaciones en la parte superior del controlador; facilita la lectura y ayuda a identificar las variables asociadas a la vista.
Evitaremos este tipo de orden:


**Debemos evitar:**

```javascript
function Sessions() {

    var vm = this;
    vm.gotoSession = function() {
        /* ... */
    };

    vm.sessions = [];

    vm.title = 'Sessions';
  }

```

**Debemos Organizar de la siguiente manera:**

```javascript
function Sessions() {

    var vm = this;

    // Variables
    vm.sessions = [];
    vm.title = 'Sessions';

    // Funciones
    vm.gotoSession = gotoSession;
    ////////////

    function gotoSession() {
        /* ... */
    }
  }
```



##Delegar la lógica dentro de los controladores a servicios o factorías

Es recomendable delegar la lógica de los controladores a servicios o factorías para reutilizar la lógica empleada. Además se puede aislar en un test unitario y elimina dependencias a la vez que esconde detalles de la implementación.

**Debemos evitar:**
```javascript
function OrderController($http, $q, config, userInfo) {
    var vm = this;

    vm.isCreditOk;

    vm.checkCredit = checkCredit;
    /////////////////

    function checkCredit() {
        var settings = {/* ... */};

        return $http.get(settings)
            .then(function(data) {
               vm.isCreditOk = vm.total <= maxRemainingAmount
            })
            .catch(function(error) {
               // Gestionar el error
            });
    };
}
```
**Debemos Organizar de la siguiente manera:**
```javascript
function OrderController(creditService) {
    var vm = this;

    vm.isCreditOk;
    vm.total = 0;

    vm.checkCredit = checkCredit;
    ///////////////////

    function checkCredit() {
       return creditService.isOrderTotalOk(vm.total)
          .then(function(isOk) { vm.isCreditOk = isOk; })
          .catch(showError);
    };
}


```


##Utiliza Snippets

Por último, cabe destacar que existen snippets basados en estas buenas prácticas que agilizan muchísimo el desarrollo de aplicaciones . Para utilizar dichos snippets en Atom, basta con ejecutar el siguiente comando:

```javascript
npm install angularjs-styleguide-snippets
```

Los snippets incluidos en ese paquete son los siguientes:

* ngcontroller // crea un controlador de Angular
* ngdirective // crea una directiva de Angular
* ngfactory // crea una factoría de Angular
* ngmodule // crea un módulode Angular
* ngservice // crea un servicio de Angular
* ngfilter // crea un filtro de Angular
