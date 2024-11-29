# Diferencia entre promesas y observables

La principal diferencia entre un observable y una promesa radica en cómo manejan la asincronía y la cantidad de valores que pueden emitir. Ambos son herramientas para trabajar con operaciones asincrónicas, pero tienen características distintas que los hacen adecuados para diferentes escenarios.

## 1. Conceptos Básicos

Promesa (Promise):

- Representa un único valor que será resuelto en el futuro (ya sea exitosamente o con un error).
- Una vez resuelta o rechazada, su estado no cambia.
- Es ideal para operaciones que solo necesitan manejar un único resultado, como la respuesta de una solicitud HTTP.

```bash
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Hola desde una promesa'), 1000);
});

promise.then((value) => console.log(value));
```

Observable:

- Representa una secuencia de valores (puede emitir múltiples valores a lo largo del tiempo).
- Es más flexible y poderoso, especialmente para flujos de datos continuos.
- Puede ser cancelado antes de completar (usando una suscripción).
- Forma parte de la biblioteca RxJS (Reactive Extensions for JavaScript), común en Angular.

```bash
import { Observable } from 'rxjs';

const observable = new Observable((observer) => {
  observer.next('Primer valor');
  setTimeout(() => observer.next('Segundo valor'), 1000);
  setTimeout(() => observer.complete(), 2000); // Finaliza el observable
});

observable.subscribe({
  next: (value) => console.log(value),
  complete: () => console.log('Completado'),
});
```

## 2. Diferencias Principales

### Cantidad de valores

observable - Pueden emitir múltiples valores en el tiempo.
promesa - Emiten un único valor.

### Ejecución

observable - Son perezosos (lazy): no se ejecutan hasta que alguien se suscribe.
promesa - Se ejecutan inmediatamente al ser creadas.

### Cancelación

observable - Se pueden cancelar llamando a unsubscribe().
promesa - No se pueden cancelar

### Manejo de errores

observable - Se maneja con error en el objeto subscribe.
promesa - Se maneja con .catch() o try-catch.

### Interoperabilidad

observable - Necesitan importar RxJS en Angular u otros entornos.
promesa - Son nativas de JavaScript.
