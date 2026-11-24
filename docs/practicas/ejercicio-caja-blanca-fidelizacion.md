# Ejercicio de caja blanca: puntos de fidelización

| Elemento | Valor |
| --- | --- |
| RA y CE | RA3 – CE b) Se han definido casos de prueba. (Ampliación: RA3 – CE f) y g)) |
| Duración estimada | 50 min (1 sesión) · ampliación opcional: 30 min |
| Lugar | Cuaderno físico (apartados a–d) · IntelliJ (ampliación) |

## Objetivos

* Dibujar el grafo de flujo de un algoritmo con un bucle y dos decisiones.
* Calcular la complejidad ciclomática V(G) por los tres métodos y comprobar que coinciden.
* Obtener un conjunto de caminos independientes y diseñar un caso de prueba para cada uno.

## Requisitos

* Haber visto en clase el grafo de flujo de las estructuras básicas (secuencia, `SI`, `MIENTRAS`).
* Conocer las tres fórmulas de V(G).

## Enunciado

Una cafetería da puntos a sus clientes por cada compra que hacen durante el mes:

* Cada compra de **más de 50 €** suma **2 puntos**; el resto de compras suma **1 punto**.
* Si al final del mes el cliente tiene **más de 10 puntos**, recibe un **regalo de 5 puntos** extra.

El siguiente pseudocódigo calcula los puntos de un cliente. Recibe un vector con los importes de las compras y el número de compras `n`:

```text
 1  FUNCIÓN calcularPuntos(importes: vector de real, n: entero): entero
 2      puntos ← 0
 3      i ← 0
 4      MIENTRAS i < n HACER
 5          SI importes[i] > 50 ENTONCES
 6              puntos ← puntos + 2
 7          SINO
 8              puntos ← puntos + 1
 9          FIN SI
10          i ← i + 1
11      FIN MIENTRAS
12      SI puntos > 10 ENTONCES
13          puntos ← puntos + 5
14      FIN SI
15      DEVOLVER puntos
16  FIN FUNCIÓN
```

Resuelve en tu cuaderno:

**a)** Dibuja el **grafo de flujo**. Numera los nodos e indica, al lado de cada uno, qué líneas del pseudocódigo agrupa. Marca los nodos predicado y etiqueta sus aristas con V y F.

**b)** Calcula la **complejidad ciclomática** V(G) por los tres métodos (regiones, aristas − nodos + 2, nodos predicado + 1). Señala en el dibujo las regiones.

**c)** Escribe un conjunto de **caminos independientes** (tantos como indique V(G)), como secuencias de nodos.

**d)** Completa la **tabla de casos de prueba**: un caso por camino, con los valores de entrada concretos y el resultado esperado.

| Caso | Camino | `importes` | `n` | Resultado esperado |
| --- | --- | --- | --- | --- |
| CP1 | | | | |
| … | | | | |

/// note | Pista
Antes de dar por bueno un camino, **comprueba que existe una entrada que lo recorra**. Algunos caminos que parecen válidos en el grafo no se pueden dar nunca en la ejecución real.
///

/// details | Ampliación (opcional, en IntelliJ)
1. En un proyecto Maven, implementa el método `public static int calcularPuntos(double[] importes)` en la clase `Fidelizacion` (usa `importes.length` en lugar de `n`).
2. Escribe una clase de prueba JUnit 5 `FidelizacionTest` con un método `@Test` por cada caso de la tabla del apartado d).
3. Ejecuta `mvn test` y comprueba que todas las pruebas pasan.
4. Haz un *commit* con el mensaje `UT03: pruebas de caja blanca de calcularPuntos` y súbelo a tu repositorio de GitHub.
///

## Entregables

* Apartados a) a d) resueltos en el cuaderno.
* (Ampliación) Enlace al repositorio con la clase, las pruebas y el *commit*.

## Entrega

Se corrige en clase en la pizarra. Si tu profesor lo pide, sube una foto de las páginas del cuaderno a la tarea de Moodle correspondiente.

## Criterios de corrección

| Apartado | Puntos | Criterio |
| --- | --- | --- |
| a) Grafo de flujo | 3 | Nodos bien agrupados (1), aristas correctas incluida la vuelta del bucle (1), predicados y etiquetas V/F (1) |
| b) Complejidad ciclomática | 2 | Tres métodos bien aplicados y coincidentes (0,5 cada uno) y regiones señaladas (0,5) |
| c) Caminos independientes | 2 | V(G) caminos, cada uno con al menos una arista nueva (1) y todos factibles (1) |
| d) Casos de prueba | 3 | Entradas que recorren el camino indicado (1,5) y resultado esperado correcto (1,5) |
| **Total** | **10** | |

La ampliación no puntúa en este ejercicio; te sirve de repaso para la PRÁCTICA 01.
