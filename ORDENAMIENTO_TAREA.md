# Tarea — Algoritmos de ordenamiento

---

## Planteamiento del problema

Se debe definir una clase que represente alguna entidad con **varios atributos numéricos**. Por ejemplo, una clase `Estudiante` con:

* `promedio` (`double`)
* `edad` (`int`)
* `anioIngreso` (`int`)

(Puede usar este ejemplo u otro de su elección — por ejemplo `Producto` con `precio` y `stock`, o `Jugador` con `puntaje` y `nivel` —, siempre que tenga al menos dos atributos numéricos.)

Sobre un arreglo (`std::vector`) de objetos de esa clase, se debe implementar una función de ordenamiento parametrizada por el atributo que se desee ordenar, usando cada uno de los siguientes métodos:

* Burbuja (*bubble sort*)
* Selección (*selection sort*)
* Inserción (*insertion sort*)
* Mezcla (*merge sort*)
* Rápido (*quick sort*)
* `std::sort` de la biblioteca estándar

El atributo por el que se ordena **no debe quedar fijo dentro de cada algoritmo**: se debe implementar una función de comparación por cada atributo numérico (por ejemplo, `porPromedio`, `porEdad`), y pasarla como parámetro a la función de ordenamiento. Así, el mismo `bubbleSort` (o cualquier otro método) sirve para ordenar por cualquier atributo, sin reescribir el algoritmo.

```cpp
bool porPromedio(const Estudiante& a, const Estudiante& b) {
    return a.promedio < b.promedio;
}

bool porEdad(const Estudiante& a, const Estudiante& b) {
    return a.edad < b.edad;
}
```

Cada función de ordenamiento recibe entonces el criterio como parámetro adicional, de forma análoga a como `std::sort` recibe un criterio de comparación:

```cpp
void bubbleSort(std::vector<Estudiante>& v,
                 bool (*comp)(const Estudiante&, const Estudiante&));

bubbleSort(v, porPromedio);   // ordena por promedio
bubbleSort(v, porEdad);       // la misma función, ordena por edad
```

---

## Ejercicio 1

Defina la clase que va a usar en el resto de la tarea, con al menos dos atributos numéricos.

1. Incluya un constructor que inicialice todos los atributos.
2. Implemente una función o método para imprimir un objeto de la clase (por ejemplo, `mostrar()` o sobrecargando `operator<<`).
3. Genere un `std::vector` con al menos 8 objetos de esa clase, con valores distintos en cada atributo numérico.
4. Implemente una función de comparación por cada atributo numérico (por ejemplo, `porPromedio`, `porEdad`), como se describe en el planteamiento del problema.

---

## Ejercicio 2

Implemente una función:

```cpp
void bubbleSort(std::vector<Estudiante>& v,
                 bool (*comp)(const Estudiante&, const Estudiante&));
```

que ordene el arreglo de objetos usando **burbuja**, comparando con el criterio recibido en `comp`.

Pruebe la función ordenando por al menos dos atributos distintos (por ejemplo, llamando primero con `porPromedio` y luego con `porEdad`), y muestre el arreglo antes y después de cada ordenamiento.

---

## Ejercicio 3

Implemente una función:

```cpp
void selectionSort(std::vector<Estudiante>& v,
                    bool (*comp)(const Estudiante&, const Estudiante&));
```

que ordene el arreglo de objetos usando **selección**, comparando con el criterio recibido en `comp`.

Explique qué cambia en la implementación respecto al Ejercicio 2, más allá de reemplazar `int` por el tipo de la clase.

---

## Ejercicio 4

Implemente una función:

```cpp
void insertionSort(std::vector<Estudiante>& v,
                    bool (*comp)(const Estudiante&, const Estudiante&));
```

que ordene el arreglo de objetos usando **inserción**, comparando con el criterio recibido en `comp`.

Explique por qué este método puede ser más eficiente que los dos anteriores cuando el arreglo ya está casi ordenado.

---

## Ejercicio 5

Implemente una función:

```cpp
void mergeSort(std::vector<Estudiante>& v, int inicio, int fin,
                bool (*comp)(const Estudiante&, const Estudiante&));
```

que ordene el arreglo de objetos usando **mezcla**, comparando con el criterio recibido en `comp`. Recuerde que `comp` debe propagarse también a la función auxiliar `merge` y a las llamadas recursivas.

Explique:

* ¿Por qué `merge sort` necesita memoria adicional (`O(n)`), a diferencia de los métodos anteriores?
* ¿Qué condición en la función `merge` garantiza que el ordenamiento sea estable?

---

## Ejercicio 6

Implemente una función:

```cpp
void quickSort(std::vector<Estudiante>& v, int inicio, int fin,
                bool (*comp)(const Estudiante&, const Estudiante&));
```

que ordene el arreglo de objetos usando **rápido** (*quick sort*), comparando con el criterio recibido en `comp`. Recuerde que `comp` debe propagarse también a la función auxiliar `partition` y a las llamadas recursivas.

Explique:

* ¿Qué es el pivote y cómo afecta la elección del pivote al desempeño del algoritmo?
* ¿Por qué este método no es estable?

---

## Ejercicio 7

Ordene el mismo arreglo usando `std::sort` de `<algorithm>`, reutilizando las mismas funciones de comparación del Ejercicio 1 (`porPromedio`, `porEdad`, etc.).

```cpp
std::sort(v.begin(), v.end(), porPromedio);
```

Explique:

* ¿Qué ventajas tiene usar `std::sort` frente a implementar el algoritmo manualmente?
* Las funciones de comparación son las mismas que usó en los Ejercicios 2 a 6. ¿Por qué es posible reutilizarlas sin cambiarlas?

---
