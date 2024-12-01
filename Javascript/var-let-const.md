# Var, Let y Const

En javascript var, let y const son palabras clave utilizadas para declarar variables. aunque cumplen funciones similares, tienen diferencias significativas en cuanto al alcance, reasignacion y hoisting.

## Var

Es la forma original de declarar variables en javascript anterior a ES6. Su uso hoy en dia no es recomendado debido a comportamiento que puede tener que nos pueden llevar a tener errores involuntarios.

### Caracteristicas de Var

Alcance(Scope) - Tiene alcance de funcion, una variable declarada con Var esta disponible dentro de toda la funcion donde se declara, no tiene alcance de bloque, esto quiere decir si esta funcion esta declarada dentro de un condicional if, se va a poder acceder por fuera de las llaves

```bash
if (true) {
    var mensaje = "Hola";
}
console.log(mensaje); // "Hola"
```

Hoisting - Las variables declaradas con Var se elevan al comienzo del contexto pero su valor inicial es undefined.

```bash
console.log(a); // undefined
var a = 10;
```

Reasignacion - Este tipo de variables se puede reasignar sin problema y redeclarar en el mismo ambito

```bash
var a = 10;
var a = 20; // No hay problema
```

## Let

Let fue introducido en ES6 y es mas seguro que var para declara variables que cambian de valor, deberia reemplzar al uso de var para valores que no son constantes.

### Caracteristicas de Let

Alcance(Scope) - Tienen alcance de bloque, si son llamadas fuera de su scope indicarian error

```bash
if (true) {
    let mensaje = "Hola";
}
console.log(mensaje); // ReferenceError: mensaje is not defined
```

Hoisting - Este tipo de variables tambien son elevadas en su declaracion, pero no estan inicializadas, esto genera un error si se intenta acceder a ellas antes de su declaracion (TDZ - temporal dead zone)

```bash
console.log(a); // ReferenceError
let a = 10;
```

Reasignacion - Estas variables se pueden reasignar pero no se pueden volver a declarar en el mismo ambito

```bash
let a = 10;
a = 20; // OK
let a = 30; // SyntaxError
```

## Const

Forman parte de ES6 de la misma forma que Let y este tipo de variables no se pueden reasignar, se les asigna un valor al momento de crearlas y es un valor constante que no puede mutar.

Alcance(Scope) - Tienen alcance de bloque, si son llamadas fuera de su scope indicarian error por igual que Let

Hoisting - Este tipo de variables tambien son elevadas en su declaracion, pero no estan inicializadas, esto genera un error si se intenta acceder a ellas antes de su declaracion (TDZ - temporal dead zone)

```bash
console.log(a); // ReferenceError
const a = 10;
```

Inmutabilidad - Const no hace que los objetos o arrays sean inmutables, solo garantiza que la referencia a ellos no va a cambiar, los arrays que se encuentran dentro de const puede agregar nuevos valores o modificar su contenido, pero no se podria reasignar un nuevo array

```bash
const arr = [1, 2, 3];
arr.push(4); // OK
console.log(arr); // [1, 2, 3, 4]

arr = [5, 6]; // TypeError
```
