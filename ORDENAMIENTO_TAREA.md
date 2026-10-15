# Tarea — Algoritmos de ordenamiento

---

## Planteamiento del problema

Se debe definir una clase que represente alguna entidad con **varios atributos numéricos**. Por ejemplo, una clase `Estudiante` con:

* `promedio` (`double`)
* `edad` (`int`)
* `anioIngreso` (`int`)

(Puede usar este ejemplo u otro de su elección — por ejemplo `Producto` con `precio` y `stock`, o `Jugador` con `puntaje` y `nivel` —, siempre que tenga al menos dos atributos numéricos.)

Sobre un arreglo (`std::vector`) de objetos de esa clase, se debe implementar una función de ordenamiento por cada uno de los siguientes métodos:

* Burbuja (*bubble sort*)
* Selección (*selection sort*)
* Inserción (*insertion sort*)
* Mezcla (*merge sort*)
* Rápido (*quick sort*)
* `std::sort` de la biblioteca estándar

Cada función debe ordenar el arreglo según **uno de los atributos numéricos** de la clase (por ejemplo, por `promedio`), dejando los demás atributos de cada objeto intactos.

---

## Ejercicio 1

Defina la clase que va a usar en el resto de la tarea, con al menos dos atributos numéricos.

1. Incluya un constructor que inicialice todos los atributos.
2. Implemente una función o método para imprimir un objeto de la clase (por ejemplo, `mostrar()` o sobrecargando `operator<<`).
3. Genere un `std::vector` con al menos 8 objetos de esa clase, con valores distintos en el atributo por el que va a ordenar.

---

## Ejercicio 2

Implemente una función:

```cpp
void bubbleSort(std::vector<Estudiante>& v);
```

que ordene el arreglo de objetos usando **burbuja**, comparando el atributo numérico elegido.

Muestre el arreglo antes y después de ordenar.

---

## Ejercicio 3

Implemente una función:

```cpp
void selectionSort(std::vector<Estudiante>& v);
```

que ordene el arreglo de objetos usando **selección**.

Explique qué cambia en la implementación respecto al Ejercicio 2, más allá de reemplazar `int` por el tipo de la clase.

---

## Ejercicio 4

Implemente una función:

```cpp
void insertionSort(std::vector<Estudiante>& v);
```

que ordene el arreglo de objetos usando **inserción**.

Explique por qué este método puede ser más eficiente que los dos anteriores cuando el arreglo ya está casi ordenado.

---

## Ejercicio 5

Implemente una función:

```cpp
void mergeSort(std::vector<Estudiante>& v, int inicio, int fin);
```

que ordene el arreglo de objetos usando **mezcla**.

Explique:

* ¿Por qué `merge sort` necesita memoria adicional (`O(n)`), a diferencia de los métodos anteriores?
* ¿Qué condición en la función `merge` garantiza que el ordenamiento sea estable?

---

## Ejercicio 6

Implemente una función:

```cpp
void quickSort(std::vector<Estudiante>& v, int inicio, int fin);
```

que ordene el arreglo de objetos usando **rápido** (*quick sort*).

Explique:

* ¿Qué es el pivote y cómo afecta la elección del pivote al desempeño del algoritmo?
* ¿Por qué este método no es estable?

---

## Ejercicio 7

Ordene el mismo arreglo usando `std::sort` de `<algorithm>`, pasando un criterio de comparación adecuado (una función, un *lambda*, o sobrecargando `operator<` en la clase).

```cpp
std::sort(v.begin(), v.end(), /* criterio de comparación */);
```

Explique:

* ¿Qué ventajas tiene usar `std::sort` frente a implementar el algoritmo manualmente?
* ¿Cómo se le indica a `std::sort` cuál atributo usar como criterio de orden?

---
