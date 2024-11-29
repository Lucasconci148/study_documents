# ¿Cómo implementar un State Management en Angular? NgRx y otros enfoques.

El State Management es crucial para aplicaciones grandes y complejas, ya que permite gestionar el estado de la aplicación de manera centralizada y predecible. En Angular, existen varias formas de manejar el estado de la aplicación, y dos de las soluciones más comunes son NgRx y otros enfoques más simples como Services + BehaviorSubject.

## 1. NgRx:

NgRx es una librería de State Management basada en el patrón Redux y diseñada específicamente para Angular. Ofrece una solución robusta, predecible y escalable para manejar el estado de la aplicación.

### Características de NgRx:

Acciones: Definen los cambios que pueden ocurrir en el estado.
Reducers: Funciones puras que determinan cómo cambia el estado en respuesta a las acciones.
Store: Almacena el estado de la aplicación de manera global.
Selectors: Permiten consultar el estado de manera eficiente.
Effects: Se utilizan para manejar efectos secundarios como llamadas a APIs.
DevTools: Permite la integración con las herramientas de desarrollo de Redux.

## 2. Services + BehaviorSubject:

### Servicio Compartido:

```bash
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class SharedService {
  private dataSubject = new Subject<string>();
  data$ = this.dataSubject.asObservable();

  sendData(data: string) {
    this.dataSubject.next(data);
  }
}
```

### Componente Hermano 1 (Emisor):

```bash
import { Component } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'app-sibling1',
  template: `<button (click)="sendData()">Enviar a Hermano</button>`,
})
export class Sibling1Component {
  constructor(private sharedService: SharedService) {}

  sendData() {
    this.sharedService.sendData('Hola desde Hermano 1');
  }
}
```

### Componente Hermano 2 (Receptor):

```bash
import { Component, OnInit } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'app-sibling2',
  template: `<p>{{ receivedData }}</p>`,
})
export class Sibling2Component implements OnInit {
  receivedData!: string;

  constructor(private sharedService: SharedService) {}

  ngOnInit() {
    this.sharedService.data$.subscribe((data) => {
      this.receivedData = data;
    });
  }
}
```
