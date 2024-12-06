# Principios SOLID

Son un conjunto de directrices de diseño que pueden ayudar a crear sistemas de software mas mantenibles, escalables y comprensibles. Aunque originalmente fueron propuestos en el contexto de programacion orientada a objetos, se pueden aplicar facilmente en javascript.
Aplicar SOLID en JavaScript implica pensar en cómo estructurar tu código para hacerlo modular, flexible y fácil de mantener. Aunque JavaScript no es un lenguaje orientado a objetos estrictamente, puedes usar clases, módulos y patrones de diseño para implementar estos principios de manera efectiva.

## S - Principio de responsabilidad unica (Single Responsibility Principle - SRP)

Cada modulo o clase debe tener una unica razon para cambiar

```bash
// Mala práctica: un módulo hace demasiadas cosas
class UserManager {
  createUser(userData) {
    // lógica para crear usuario
  }

  validateUser(userData) {
    // lógica para validar datos del usuario
  }

  sendWelcomeEmail(userEmail) {
    // lógica para enviar un email
  }
}

// Buena práctica: separa responsabilidades
class UserCreator {
  createUser(userData) {
    // lógica para crear usuario
  }
}

class UserValidator {
  validateUser(userData) {
    // lógica para validar datos del usuario
  }
}

class EmailService {
  sendWelcomeEmail(userEmail) {
    // lógica para enviar un email
  }
}
```

## O - Principio abierto/cerrado (Open/Closed Principle - OCP)

El codigo debe estar abierto para la extension y cerrado para la modificacion.
Esto significa poder añadir nuevas funcionalidades sin tener que modificar el codigo ya existente.

```bash
// Mala práctica: modificar el código para cada nuevo descuento
class Discount {
  getDiscount(type) {
    if (type === 'student') return 20;
    if (type === 'teacher') return 10;
    return 0;
  }
}

// Buena práctica: extender comportamiento sin modificar el existente
class Discount {
  getDiscount() {
    return 0;
  }
}

class StudentDiscount extends Discount {
  getDiscount() {
    return 20;
  }
}

class TeacherDiscount extends Discount {
  getDiscount() {
    return 10;
  }
}

function calculateDiscount(discount) {
  return discount.getDiscount();
}
```

## L - Principio de sustitución de Liskov (Liskov Substitution Principle - LSP)

Los objetos de una clase derivada deben poder sustituir a los objetos de su clase base sin alterar el correcto funcionamiento del programa.

```bash
// Mala práctica: las clases derivadas no cumplen el contrato de la clase base
class Bird {
  fly() {
    console.log('Flying');
  }
}

class Penguin extends Bird {
  fly() {
    throw new Error('Penguins cannot fly');
  }
}

// Buena práctica: reorganiza la jerarquía
class Bird {}

class FlyingBird extends Bird {
  fly() {
    console.log('Flying');
  }
}

class Penguin extends Bird {
  swim() {
    console.log('Swimming');
  }
}
```

## I - Principio de segregación de interfaces (Interface Segregation Principle - ISP)

Una clase no debe verse obligada a implementar interfaces que no utiliza. En javascript, aunque no hay interfaces como tal podemos simular este principio dividiendo responsabilidades en diferentes modulos o funciones.

```bash
// Mala práctica: un objeto tiene métodos que no utiliza
class Animal {
  walk() {}
  fly() {}
}

class Dog extends Animal {
  fly() {
    throw new Error('Dogs cannot fly');
  }
}

// Buena práctica: separar interfaces
class Walkable {
  walk() {
    console.log('Walking');
  }
}

class Flyable {
  fly() {
    console.log('Flying');
  }
}

class Dog extends Walkable {}
class Bird extends Walkable {
  fly() {
    console.log('Flying');
  }
}
```

## D - Principio de inversión de dependencias (Dependency Inversion Principle - DIP)

Las clases de alto nivel no deberian depender de clases de bajo nivel. Ambas deberian depender de abstracciones.

```bash
// Mala práctica: la clase depende directamente de otra clase concreta
class MySQLDatabase {
  connect() {
    console.log('Connecting to MySQL');
  }
}

class UserService {
  constructor() {
    this.database = new MySQLDatabase(); // Dependencia directa
  }

  getUser() {
    this.database.connect();
    console.log('Fetching user');
  }
}

// Buena práctica: usa una abstracción (como una interfaz o un contrato)
class UserService {
  constructor(database) {
    this.database = database; // Dependencia inyectada
  }

  getUser() {
    this.database.connect();
    console.log('Fetching user');
  }
}

class MySQLDatabase {
  connect() {
    console.log('Connecting to MySQL');
  }
}

class MongoDB {
  connect() {
    console.log('Connecting to MongoDB');
  }
}

// Uso
const database = new MySQLDatabase();
const userService = new UserService(database);
userService.getUser();
```
