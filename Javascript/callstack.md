# Call Stack o Pila de ejecucion

La pila de ejecucion en javascript es la forma en el que el motor de javascript va a ejecutar nuestros programas o aplicaciones. Javascript puede ejecutar una sola tarea a la vez por esto mismo utiliza una pila o una estructura de datos tipo LIFO (ultimo en entrar, primero en salir) para ir ejecutando las distintas instrucciones que tiene nuestro programa.

En la pila de ejecucion podemos ir viendo como las llamadas a las funciones se van apilando y ejecutando y una vez que se completan se quitan de la pila, de esta forma javascript puede saber cual era la funcion que invoco a la que acaba de terminar para poder continuar con la ejecucion del programa.

Cuando tenemos un error en la ejecucion de nuestro programa, el motor de javascript nos va a mostrar por consola desde donde se desencadeno el error mostrandono toda la pila de ejecucion.

La pila de ejecucion dentro de los navegadores contienen un limite, esto quiere decir que si nuestra pila entra en un bucle infinito o se crean demasiados registros para nuestra pila de ejecucion, el motor de javascript se detendra indicando que superamos el limite y frenando nuestro programa.

## Funcionamiento de la pila de ejecucion

El motor de javascript lee el codigo de nuestro programa y cada vez que encuentre con el llamado o ejecucion de una funcion crea un registro para la Pila de ejecucion, este registro contiene toda la informacion necesaria para poder ejecutar esta funcion, de esta manera el registro que queda en la parte superior de la pila es la funcion que se esta ejecutando en este momento.

### Ejemplo simple

```bash
function funcion1() {
    console.log("funcion1");
    funcion2();
    console.log("funcion1 terminada");
}

function funcion2() {
    console.log("funcion2");
}

funcion1();
```

En el ejemplo anterior Javascript creara un nuevo registro para la pila con el llamado a la funcion1, al empezar a ejecutar la funcion creara un nuevo registro para la funcion2 que se encuentra dentro del cuerpo de la funcion1 y ejecutara la funcion dos, en este momento la pila de ejecucion contiene ambas funciones, al finalizar la ejecucion de la funcion2 javascript eliminara este registro de la pila y continuara con la ultima salida por consola de la funcion1 y con esto eliminara la funcion1 de la pila. Quedando vacia la pila de ejecucion el programa queda completado.

### Ejemplo simple con error

```bash
function funcion1() {
    console.log("funcion1");
    funcion2();
    console.log("funcion1 terminada");
}

function funcion2() {
    throw new Error("Error")
}

funcion1();
```

En el ejemplo anterior, tenemos un error dentro de la funcion2. Esto va a desencadenar un mensaje de error que javascript nos va a mostrar por la consola y nos va a mostrar el callstack completo, donde fue que se desencadeno el error.

## Registro que se incluye a la pila

## Funcionamiento de la pila de ejecucion con asincronismo
