# Algoritmos de ordenamiento en C++

*Prof. Jose Francisco Ruiz Muñoz*<br>
*Programación Avanzada 2026-II*<br>
*Universidad Nacional de Colombia - Sede de La Paz*<br>

Ordenar es reorganizar los elementos de una colección (un arreglo, un `std::vector`, etc.) según un criterio, típicamente de menor a mayor. Existen muchas formas de resolver este problema; en esta guía se presentan los algoritmos clásicos, su implementación en C++ y los criterios para comparar su desempeño.

---

## 1. Criterios para comparar algoritmos de ordenamiento

Antes de ver los algoritmos, conviene conocer los criterios con los que se comparan:

* **Complejidad temporal**: cuántas operaciones realiza el algoritmo en función del tamaño de la entrada `n`, típicamente expresada en notación *Big O*. Se distingue entre **mejor caso**, **caso promedio** y **peor caso**.
* **Complejidad espacial**: cuánta memoria adicional necesita el algoritmo, más allá de la colección original. Un algoritmo **in-place** (en el sitio) usa solo una cantidad constante de memoria extra.
* **Estabilidad**: un algoritmo es **estable** si conserva el orden relativo de los elementos que son considerados iguales según el criterio de comparación. Esto importa, por ejemplo, si ya se ordenó una lista de estudiantes por nombre y luego se quiere ordenar por curso: un algoritmo estable no altera el orden alfabético dentro de cada curso.

---

## 2. Bubble sort (ordenamiento de burbuja)

Recorre repetidamente el arreglo comparando elementos adyacentes e intercambiándolos si están en el orden incorrecto. En cada pasada, el elemento más grande "burbujea" hacia el final.

```cpp
void bubbleSort(std::vector<int>& v) {
    int n = v.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (v[j] > v[j + 1]) {
                std::swap(v[j], v[j + 1]);
            }
        }
    }
}
```

* **Complejidad**: mejor caso `O(n)` (con una bandera que detecte si no hubo intercambios), caso promedio y peor caso `O(n²)`.
* **Espacio extra**: `O(1)`, in-place.
* **Estable**: sí, porque solo intercambia elementos adyacentes cuando uno es estrictamente mayor que el otro.

---

## 3. Selection sort (ordenamiento por selección)

En cada pasada busca el elemento más pequeño del resto del arreglo y lo coloca en su posición final mediante un único intercambio.

```cpp
void selectionSort(std::vector<int>& v) {
    int n = v.size();
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (v[j] < v[minIdx]) {
                minIdx = j;
            }
        }
        std::swap(v[i], v[minIdx]);
    }
}
```

* **Complejidad**: `O(n²)` en todos los casos (siempre recorre el resto del arreglo, incluso si ya está ordenado).
* **Espacio extra**: `O(1)`, in-place.
* **Estable**: no, en su forma clásica. El intercambio puede mover un elemento más allá de otro igual a él (se puede hacer estable insertando en vez de intercambiar, a costa de más movimientos).

---

## 4. Insertion sort (ordenamiento por inserción)

Construye el resultado ordenado de a un elemento a la vez: toma cada elemento y lo inserta en la posición correcta dentro de la parte ya ordenada del arreglo.

```cpp
void insertionSort(std::vector<int>& v) {
    int n = v.size();
    for (int i = 1; i < n; i++) {
        int clave = v[i];
        int j = i - 1;
        while (j >= 0 && v[j] > clave) {
            v[j + 1] = v[j];
            j--;
        }
        v[j + 1] = clave;
    }
}
```

* **Complejidad**: mejor caso `O(n)` (arreglo ya ordenado), caso promedio y peor caso `O(n²)`.
* **Espacio extra**: `O(1)`, in-place.
* **Estable**: sí, porque un elemento solo se desplaza cuando el anterior es estrictamente mayor.
* Es eficiente para arreglos pequeños o casi ordenados; por eso algunas implementaciones híbridas (como `std::sort`) lo usan para los últimos pasos.

---

## 5. Merge sort (ordenamiento por mezcla)

Aplica la estrategia **divide y vencerás**: divide el arreglo por la mitad recursivamente hasta llegar a subarreglos de un elemento (trivialmente ordenados), y luego los va **mezclando** de a pares en orden.

```cpp
void merge(std::vector<int>& v, int inicio, int medio, int fin) {
    std::vector<int> izq(v.begin() + inicio, v.begin() + medio + 1);
    std::vector<int> der(v.begin() + medio + 1, v.begin() + fin + 1);

    size_t i = 0, j = 0;
    int k = inicio;
    while (i < izq.size() && j < der.size()) {
        v[k++] = (izq[i] <= der[j]) ? izq[i++] : der[j++];
    }
    while (i < izq.size()) v[k++] = izq[i++];
    while (j < der.size()) v[k++] = der[j++];
}

void mergeSort(std::vector<int>& v, int inicio, int fin) {
    if (inicio >= fin) return;   // 0 o 1 elemento: ya está ordenado

    int medio = inicio + (fin - inicio) / 2;
    mergeSort(v, inicio, medio);
    mergeSort(v, medio + 1, fin);
    merge(v, inicio, medio, fin);
}
```

Uso:

```cpp
std::vector<int> v = {5, 2, 4, 1, 3};
mergeSort(v, 0, v.size() - 1);
```

* **Complejidad**: `O(n log n)` en mejor, promedio y peor caso — el trabajo de dividir es `log n` niveles, y cada nivel mezcla `n` elementos en total.
* **Espacio extra**: `O(n)`, porque `merge` necesita arreglos auxiliares. No es in-place.
* **Estable**: sí, siempre que `merge` use `<=` (y no `<`) al elegir entre `izq[i]` y `der[j]`, como en el código de arriba.

---

## 6. Quick sort (ordenamiento rápido)

También usa divide y vencerás, pero de forma distinta: elige un **pivote**, reordena el arreglo (*partición*) para que los elementos menores queden a su izquierda y los mayores a su derecha, y luego ordena recursivamente cada lado. A diferencia de merge sort, no necesita mezclar al final.

```cpp
int partition(std::vector<int>& v, int inicio, int fin) {
    int pivote = v[fin];
    int i = inicio - 1;

    for (int j = inicio; j < fin; j++) {
        if (v[j] < pivote) {
            i++;
            std::swap(v[i], v[j]);
        }
    }
    std::swap(v[i + 1], v[fin]);
    return i + 1;
}

void quickSort(std::vector<int>& v, int inicio, int fin) {
    if (inicio >= fin) return;

    int p = partition(v, inicio, fin);
    quickSort(v, inicio, p - 1);
    quickSort(v, p + 1, fin);
}
```

* **Complejidad**: caso promedio `O(n log n)`; peor caso `O(n²)`, que ocurre cuando el pivote elegido queda siempre en un extremo (por ejemplo, en un arreglo ya ordenado con esta versión del pivote). En la práctica se mitiga eligiendo el pivote de forma aleatoria o como la mediana de unos pocos candidatos.
* **Espacio extra**: `O(log n)` por la pila de llamadas recursivas (in-place en el arreglo mismo, a diferencia de merge sort).
* **Estable**: no. La partición intercambia elementos que pueden saltarse otros iguales a ellos.

---

## 7. Comparación de algoritmos

| Algoritmo      | Mejor caso   | Caso promedio | Peor caso    | Espacio extra | Estable |
| -------------- | ------------ | -------------- | ------------ | -------------- | ------- |
| Bubble sort    | `O(n)`       | `O(n²)`        | `O(n²)`      | `O(1)`         | Sí      |
| Selection sort | `O(n²)`      | `O(n²)`        | `O(n²)`      | `O(1)`         | No      |
| Insertion sort | `O(n)`       | `O(n²)`        | `O(n²)`      | `O(1)`         | Sí      |
| Merge sort     | `O(n log n)` | `O(n log n)`   | `O(n log n)` | `O(n)`         | Sí      |
| Quick sort     | `O(n log n)` | `O(n log n)`   | `O(n²)`      | `O(log n)`     | No      |

---

## 8. ¿Qué usar en la práctica?

En C++ moderno casi nunca se implementan estos algoritmos a mano en código de producción: la biblioteca estándar ya ofrece versiones altamente optimizadas.

```cpp
#include <algorithm>
#include <vector>

std::vector<int> v = {5, 2, 4, 1, 3};

std::sort(v.begin(), v.end());          // rápido, pero no garantiza estabilidad
std::stable_sort(v.begin(), v.end());   // garantiza estabilidad (usa merge sort internamente)
```

* `std::sort` suele implementarse como **introsort** (una combinación de quick sort, heap sort e insertion sort para casos pequeños), y ofrece `O(n log n)` en el peor caso.
* `std::stable_sort` garantiza estabilidad, típicamente a costa de memoria adicional.

Conocer bubble sort, selection sort e insertion sort sigue siendo valioso para entender los fundamentos (comparación, intercambio, complejidad) antes de estudiar algoritmos más eficientes como merge sort y quick sort, que son la base de las implementaciones reales.

---

**Este repositorio fue desarrollado con apoyo de inteligencia artificial. El contenido fue revisado, validado y editado cuidadosamente por el docente responsable.*
