# ¿Cómo implementar la comunicación entre componentes en Angular?

En Angular, los componentes son unidades independientes que pueden comunicarse entre sí para compartir datos o eventos. La comunicación entre componentes depende de su relación jerárquica (padre-hijo, hijo-padre, componentes hermanos o no relacionados). A continuación, se explican los métodos más comunes para lograr esta comunicación:

1. Comunicación de Padre a Hijo:

Utiliza decoradores de entrada (@Input) para pasar datos desde un componente padre a uno hijo.

### Componente Padre:

```bash
 import { Component } from '@angular/core';

@Component({
 selector: 'app-parent',
 template: `<app-child [childData]="data"></app-child>`,
})
export class ParentComponent {
 data = 'Hola desde el componente padre';
}
```

### Componente Hijo:

```bash
import { Component, Input } from '@angular/core';

@Component({
 selector: 'app-child',
 template: `<p>{{ childData }}</p>`,
})
export class ChildComponent {
 @Input() childData!: string;
}
```

2. Comunicación de Hijo a Padre:

Utiliza decoradores de salida (@Output) y un EventEmitter para enviar datos o eventos desde el hijo al padre.

### Componente Hijo:

```bash
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<button (click)="sendData()">Enviar al Padre</button>`,
})
export class ChildComponent {
  @Output() dataEmitter = new EventEmitter<string>();

  sendData() {
    this.dataEmitter.emit('Hola desde el componente hijo');
  }
}
```

### Componente Padre:

```bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `<app-child (dataEmitter)="receiveData($event)"></app-child>
             <p>{{ receivedData }}</p>`,
})
export class ParentComponent {
  receivedData!: string;

  receiveData(data: string) {
    this.receivedData = data;
  }
}
```

3. Comunicación entre Componentes Hermanos:

Los componentes hermanos no tienen una relación jerárquica directa. Para comunicarse, suelen compartir un servicio que actúa como intermediario. En el siguiente ejemplo usamos un servicio que es una instancia unica en el proyecto y enviamos la informacion a travez de un subject

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

4. Comunicación entre Componentes No Relacionados:

Utiliza un servicio global similar al caso anterior o un Event Bus. Otra opción es usar State Management con bibliotecas como NgRx o Akita.
