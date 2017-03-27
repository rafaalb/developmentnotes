Para automatizar las tareas usamos gulp o grunt. Gulp se enfoca en codear la configuracion de paquetes y grunt se enfoca en configurar para la automatizacion. Para los proyectos usamos Grunt porque el codigo es mas facil de leer y escribir, pero ambos son excelentes.

 Para automatizar tareas al listar la definicion de los modulos de javascript  *.module.js antes que cualquier otro archivo javascript.

Porque?: Angular necesita definir los modulos antes de ser utilizados.

Ademas de que nombrar los modulos con la extension *.module.js  da mayor facilidad al englobar y listarlos dichos archivos de primero.

var clientApp = './src/client/app/';

var files = [
  clientApp + '**/*.module.js',
  clientApp + '**/*.js'
];

Debemos contar con un build para el ambiente de pruebas QA y uno distinto para ambiente de desarrollo.
