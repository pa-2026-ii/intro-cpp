# Guía breve de programación en C++

*Prof. Jose Francisco Ruiz Muñoz*<br>
*Programación Avanzada 2026-II*<br>
*Universidad Nacional de Colombia - Sede de La Paz*<br>

El lenguaje C++ extiende el lenguaje C incorporando abstracciones de alto nivel como programación orientada a objetos, genéricos (templates) y manejo más seguro de recursos. Es ampliamente utilizado en sistemas de alto rendimiento, videojuegos, simulación científica y software industrial.

---

## 1. El lenguaje C++ y el modelo multiparadigma

C++ es un lenguaje:

* Compilado
* Imperativo
* De tipado estático
* Multiparadigma (procedimental, orientado a objetos y genérico)

A diferencia de C, permite construir **abstracciones de alto nivel** sin perder control sobre la memoria.

---

## 2. Variables y tipos de datos

### 2.1 Definición de variables

```cpp
int x = 10;
double y{3.14};   // inicialización uniforme
```

C++ permite múltiples formas de inicialización:

* Inicialización clásica (`=`)
* Inicialización directa (`()`)
* Inicialización uniforme (`{}`)

---

### 2.2 Tipos básicos

| Tipo          | Descripción          |
| ------------- | -------------------- |
| `int`         | Entero               |
| `double`      | Punto flotante doble |
| `char`        | Carácter             |
| `bool`        | Booleano             |
| `std::string` | Cadena de texto      |

Ejemplo:

```cpp
#include <string>
std::string nombre = "Jose";
```
---

---

### 2.3 Formas de inicialización en C++

C++ permite varias formas de inicializar variables. Aunque todas asignan un valor inicial, **no son completamente equivalentes**.

---

#### 1. Inicialización clásica (`=`)

```cpp
int x = 10;
double y = 3.14;
```

Se llama *copy initialization*.
En tipos simples funciona como una asignación al momento de crear la variable.

Permite conversiones implícitas:

```cpp
int a = 3.7;   // permitido
```

Aquí `3.7` se convierte a `3`.
Se pierde la parte decimal sin que el compilador lo impida.

---

#### 2. Inicialización directa (`()`)

```cpp
int x(10);
double y(3.14);
```

Se llama *direct initialization*.
Invoca directamente el constructor (importante en clases).

También permite conversiones implícitas:

```cpp
int a(3.7);   // permitido
```

De nuevo, se pierde información.

---

#### 3. Inicialización uniforme (`{}`)

Introducida en C++11.

```cpp
int x{10};
double y{3.14};
```

Es la forma recomendada en C++ moderno porque **evita conversiones peligrosas**.

Ejemplo:

```cpp
int a{3.7};   // ERROR de compilación
```

Aquí el compilador detecta que:

* `3.7` es `double`
* `int` no puede representarlo exactamente
* La conversión perdería información

Y bloquea el programa.

---

### ¿Qué es narrowing?

Una conversión *narrowing* es aquella que:

* Reduce el rango o la precisión del valor
* Puede perder información

Ejemplos:

```cpp
double → int      // pierde decimales
long long → int   // puede perder rango
float → int       // pierde precisión
```

La inicialización con `{}` impide este tipo de conversiones.

---

### Recomendación práctica

En C++ moderno se recomienda preferir `{}` porque:

* Es más segura
* Evita errores silenciosos
* Hace explícitas las conversiones que podrían perder información

---

## 3. Estructuras de control

### 3.1 Selección

```cpp
if (x > y) {
    max = x;
} else {
    max = y;
}
```

Similar a C, pero con tipos booleanos explícitos (`bool`).

---

### 3.2 Iteración

```cpp
for (int i = 0; i < 5; ++i) {
    suma += i;
}
```

También existe el **range-based for**:

```cpp
int arr[] = {1,2,3};
for (int v : arr) {
    std::cout << v << std::endl;
}
```

---

## 4. Funciones

```cpp
int suma(int a, int b) {
    return a + b;
}
```

C++ permite:

**Sobrecarga de funciones** — varias funciones con el mismo nombre y distintos parámetros:

```cpp
int suma(int a, int b) { return a + b; }
double suma(double a, double b) { return a + b; }
// suma(1, 2) → 3  ;  suma(1.5, 2.5) → 4.0
```

**Parámetros por referencia** — el parámetro es un alias del argumento (se puede modificar el original):

```cpp
void incrementar(int &x) {
    x++;
}
// int n = 5; incrementar(n);  → n vale 6
```

**Valores por defecto** — los últimos parámetros pueden tener un valor si no se pasan:

```cpp
void saludar(const std::string& nombre, int veces = 1) {
    for (int i = 0; i < veces; i++)
        std::cout << "Hola, " << nombre << "\n";
}
// saludar("Ana");     → imprime una vez
// saludar("Ana", 3); → imprime tres veces
```

## 4.1 Paso por valor vs paso por referencia

En C++, los parámetros pueden pasarse:

### Por valor (se crea una copia)

```cpp
void incrementar(int x) {
    x++;
}
```

Aquí `x` es una copia.
La variable original no cambia.

---

### Por referencia (se modifica el original)

```cpp
void incrementar(int &x) {
    x++;
}
```

Aquí `x` es un alias del argumento original.

Diferencia conceptual:

* Valor → se copia
* Referencia → se modifica el objeto original

---


---

## 5. Programación orientada a objetos

### 5.1 Definición de clase

```cpp
class Persona {
public:
    std::string nombre;
    int edad;

    void saludar() {
        std::cout << "Hola\n";
    }
};
```

Uso:

```cpp
Persona p;
p.nombre = "Ana";
p.saludar();
```

---

## 6. Manejo de memoria

### 6.0 Stack vs. heap

Un programa organiza su memoria en distintas zonas. Dos de las más relevantes son:

* **Stack (pila)**: memoria automática. Las variables locales se reservan y liberan solas al entrar y salir de un bloque `{ }`. Es rápida, pero su tamaño es limitado y la vida de las variables está atada al alcance donde se declaran.
* **Heap (montículo)**: memoria dinámica. El programador la reserva explícitamente con `new` y debe liberarla con `delete`. Permite crear objetos cuyo tamaño no se conoce en tiempo de compilación, o que deben seguir existiendo más allá del bloque donde se crearon.

```cpp
void f() {
    int a = 10;              // en el stack: se libera sola al salir de f()

    int* p = new int(20);    // en el heap: persiste hasta hacer delete
    delete p;                // liberación manual
}
```

Como la memoria del heap no se libera automáticamente, es responsabilidad del programador hacerlo; si se olvida, ocurre una **fuga de memoria** (ver sección 9, punto 4).

**¿Cuándo usar cada uno?**

Regla general: usar el stack por defecto, y el heap solo cuando se necesite explícitamente.

Usar el **stack** cuando:

* El tamaño del objeto se conoce en tiempo de compilación.
* El objeto solo necesita existir dentro del bloque/función donde se crea.
* Se quiere simplicidad y rendimiento (no hay que llamar a `delete` ni hay riesgo de fugas).

Usar el **heap** cuando:

1. **El tamaño no se conoce hasta tiempo de ejecución** (por ejemplo, depende de una entrada del usuario).
2. **El objeto debe sobrevivir más allá del bloque donde se crea.**
3. **El objeto es muy grande**: el stack tiene un tamaño limitado (unos pocos MB); estructuras grandes suelen ir al heap para evitar un *stack overflow*.
4. **Se necesita ownership compartido o polimorfismo** (por ejemplo, guardar objetos de distintas subclases a través de un puntero base).

En C++ moderno casi nunca se usa `new`/`delete` manualmente. Se prefieren estructuras que gestionan el heap automáticamente: `std::vector`, `std::string` (tamaño dinámico) y punteros inteligentes como `std::unique_ptr` (RAII, ver más abajo en esta sección).

---

En C++ clásico:

```cpp
int* p = new int(5);
delete p;
```

Aquí `new int(5)` reserva memoria para **un solo entero**, inicializado con el valor `5`. El `(5)` no es un tamaño: no crea un arreglo. Para reservar un arreglo dinámico se usan corchetes:

```cpp
int* arr = new int[5];    // arreglo de 5 enteros (valores indeterminados)
delete[] arr;             // los arreglos se liberan con delete[], no delete
```

Usar `delete` en un puntero reservado con `new[]` (o viceversa) es comportamiento indefinido.

En C++ moderno se recomienda usar **RAII** (Resource Acquisition Is Initialization) y punteros inteligentes:

- **RAII**: el recurso (memoria) se adquiere al crear el objeto y se libera automáticamente cuando el objeto sale de alcance. No hace falta llamar a `delete` a mano.
- **Punteros inteligentes**: `std::unique_ptr` es dueño exclusivo del objeto; cuando el puntero se destruye, libera la memoria.

```cpp
#include <memory>
std::unique_ptr<int> p = std::make_unique<int>(5);
// Al salir del bloque, p se destruye y la memoria se libera sola
```

**Salir del bloque** significa que la ejecución abandona el trozo de código entre llaves `{ }`. En ese momento las variables declaradas dentro se destruyen. Un bloque es todo lo que está entre una `{` y su `}` (el cuerpo de una función, un `if`, un `for`, o unas llaves puestas solo para limitar alcance). Ejemplo:

```cpp
{
    std::unique_ptr<int> p = std::make_unique<int>(5);
    // aquí p existe
}   // al llegar aquí, p se destruye y se libera la memoria
```

Así se evitan fugas de memoria (olvidar `delete`) y dobles liberaciones.

---

---

## 6.1 Uso básico de punteros seguros

En C++ moderno se recomienda:

```cpp
int* p = nullptr;
```

Antes de usar un puntero se debe verificar:

```cpp
if (p != nullptr) {
    // usar p
}
```

**¿Por qué inicializar en `nullptr`?** Si se declara un puntero sin darle un valor (`int* p;`), el compilador no le asigna `nullptr` automáticamente: la variable queda con lo que hubiera antes en esa zona de memoria. Ese puntero "sin inicializar" puede apuntar a cualquier dirección, y usarlo (`*p`) es comportamiento indefinido. Por eso siempre se inicializa a `nullptr` y se verifica con `if (p != nullptr)` antes de usarlo.

---


---

## 6.2 Alcance (scope) de variables

Las variables solo existen dentro del bloque donde se declaran.

```cpp
{
    int x = 10;
}
// aquí x ya no existe
```

Intentar usar una variable fuera de su alcance produce error.

---


## 7. Templates (Programación genérica)

```cpp
template <typename T>
T suma(T a, T b) {
    return a + b;
}
```

Uso con distintos tipos (el compilador genera la versión adecuada en cada caso):

```cpp
suma(3, 5);        // int → 8
suma(2.5, 1.5);    // double → 4.0
suma(1.0f, 2.0f); // float → 3.0f
```

Permite escribir código independiente del tipo.

---

## 8. Relación con bajo nivel

C++ mantiene compatibilidad con C:

* Uso de punteros
* Control explícito de memoria
* Traducción eficiente a código máquina

Sin embargo, incorpora abstracciones que el compilador optimiza sin costo adicional cuando se usan correctamente.

---

## 9. Errores comunes

### 1. No inicializar variables

```cpp
int x;
std::cout << x;   // valor indefinido
```

Las variables locales no se inicializan automáticamente.
Siempre deben inicializarse.

Solución: usar inicialización directa o uniforme.

---

### 2. Confundir paso por valor con paso por referencia

```cpp
void f(int x) {
    x = 100;
}
```

El valor original no cambia porque se creó una copia.

Si se desea modificar el argumento original, usar referencia:

```cpp
void f(int &x)
```

---

### 3. Usar punteros sin inicializar

```cpp
int* p;
*p = 10;   // comportamiento indefinido
```

Inicializar siempre:

```cpp
int* p = nullptr;
```

Y verificar antes de usar.

---

### 4. No liberar memoria al usar `new`

```cpp
int* p = new int(5);
// si no se hace delete, hay fuga de memoria
```

`new int(5)` reserva un entero en el heap con valor inicial `5`. Esa memoria queda reservada hasta que se llame explícitamente a `delete p;`. La fuga de memoria ocurre si el programa pierde la referencia a `p` (por ejemplo, `p` sale de alcance, o se le reasigna otro valor) **antes** de liberar esa memoria: ya no hay forma de acceder a ella para hacer `delete`, y el sistema operativo no la recupera hasta que el programa termina.

**¿Qué es una fuga de memoria (memory leak)?**

Una fuga de memoria ocurre cuando un programa reserva memoria dinámicamente (con `new`, por ejemplo) y pierde toda forma de acceder a ella sin haberla liberado con `delete`. La memoria queda ocupada desde el punto de vista del sistema operativo, pero el programa ya no tiene ningún puntero hacia ella.

Otra forma común de generar una fuga es reasignar el puntero sin liberar lo anterior:

```cpp
int* p = new int(5);
p = new int(10);   // fuga: se perdió la referencia al primer int(5)
delete p;           // solo libera el segundo, el primero quedó perdido
```

Si esto se repite muchas veces (por ejemplo, dentro de un bucle o en un programa de larga duración como un servidor), el programa consume cada vez más memoria RAM, lo que puede degradar el rendimiento o llegar a agotar la memoria disponible.

Mejor práctica en C++ moderno:

```cpp
std::unique_ptr<int> p = std::make_unique<int>(5);
```

Evita fugas automáticamente.

---

### 5. No usar `{}` cuando hay riesgo de narrowing

```cpp
int x = 3.7;   // pierde información
```

Preferir:

```cpp
int x{3};  // seguro
```

El compilador evita conversiones peligrosas.

---

---

## TAREA

La tarea de este tema se encuentra en el archivo [TAREA.md](TAREA.md).

---

**Este repositorio fue desarrollado con apoyo de inteligencia artificial. El contenido fue revisado, validado y editado cuidadosamente por el docente responsable.*
