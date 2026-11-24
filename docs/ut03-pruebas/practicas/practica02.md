# PRÁCTICA 02: CAJA BLANCA — CAMINO BÁSICO, JUNIT Y COBERTURA

---

| | |
| --- | --- |
| **Resultado de aprendizaje** | RA3. Verifica el funcionamiento de programas diseñando y realizando pruebas. |
| **Criterios de evaluación** | RA3 – CE b) Se han definido casos de prueba · RA3 – CE f) Se han efectuado pruebas unitarias de clases y funciones · RA3 – CE g) Se han implementado pruebas automáticas |
| **Duración** | 2 sesiones en el aula + 1 h en casa |
| **Modalidad** | Individual |

## Objetivos

* Aplicar la técnica del **camino básico**: grafo de flujo, complejidad ciclomática, caminos independientes y casos de prueba.
* Implementar en **JUnit 5** una prueba por cada camino independiente.
* Comprobar con la herramienta de **cobertura** del IDE que las pruebas recorren todo el código.
* Combinar caja blanca y caja negra en un mismo método.

## Requisitos

* IntelliJ IDEA con JDK 21, Maven y Git.
* Haber hecho EC3 y la PRÁCTICA 01.

---

## Enunciado

Vas a probar la clase `AnalisisArrays`, que contiene dos métodos:

```java
package es.albares.eedd.ut03;

public class AnalisisArrays {

    /**
     * Devuelve el mayor valor del array.
     * @throws IllegalArgumentException si el array es null o está vacío
     */
    public static int buscarMaximo(int[] v) {
        if (v == null || v.length == 0) {
            throw new IllegalArgumentException("El array no puede estar vacío");
        }
        int max = v[0];
        for (int i = 1; i < v.length; i++) {
            if (v[i] > max) {
                max = v[i];
            }
        }
        return max;
    }

    /**
     * Cuenta cuántas notas están aprobadas (entre 5 y 10, ambos incluidos).
     * Las notas fuera del rango [0, 10] se ignoran.
     */
    public static int contarAprobados(double[] notas) {
        int aprobados = 0;
        for (double nota : notas) {
            if (nota >= 5 && nota <= 10) {
                aprobados++;
            }
        }
        return aprobados;
    }
}
```

### Parte A. Camino básico (a mano, en el cuaderno)

Para **cada método**:

1. Copia el código y **numera los nodos** siguiendo las reglas de la teoría (apartado 6.3). Recuerda: cada condición simple es un nodo predicado y todas las salidas (`return`, `throw`) deben llegar a un **único nodo final**.
2. Dibuja el **grafo de flujo**, marcando las aristas V y F de cada nodo predicado.
3. Calcula **V(G) con las tres fórmulas** (regiones, A − N + 2 y P + 1). Deben coincidir.
4. Escribe los **caminos independientes** como secuencias de nodos.
5. Diseña un **caso de prueba por camino**: entrada concreta y resultado esperado.

Haz una foto clara de cada página: irán en la memoria.

### Parte B. Proyecto y pruebas JUnit

1. Crea un repositorio privado `eedd-ut03-practica02` en GitHub (invita al profesor) y un proyecto Maven `ut03-practica02` con el `pom.xml` de la teoría.
2. Añade `AnalisisArrays` y haz el primer commit.
3. Crea `AnalisisArraysTest` con **una prueba `@Test` por cada camino independiente** de `buscarMaximo`. Cada prueba debe llevar un `@DisplayName` que empiece por el camino que recorre, por ejemplo `@DisplayName("C3: array con un solo elemento")`.
4. Haz lo mismo para `contarAprobados`. Aquí puedes agrupar los caminos en una prueba con `assertAll` o hacer una prueba por camino.
5. Commit: `Pruebas de caminos independientes`.

### Parte C. Caja negra sobre el mismo método

Los caminos independientes de `contarAprobados` no garantizan que los **límites** estén bien. Añade una prueba parametrizada con los valores límite de la condición `nota >= 5 && nota <= 10`, usando arrays de una sola nota (por ejemplo `4.99`, `5`, `10` y `10.01`) con su resultado esperado.

Commit: `Pruebas de valores límite de contarAprobados`.

### Parte D. Cobertura y automatización

1. Ejecuta `AnalisisArraysTest` **con cobertura**. El objetivo es **100 % de líneas y de ramas** en `AnalisisArrays`. Si no llegas, revisa qué camino te falta.
2. Ejecuta `mvn test` en la terminal.
3. Contesta en la memoria: si `buscarMaximo` recibiera también un parámetro `boolean ignorarNegativos` y añadieras dentro del bucle un `if (ignorarNegativos && v[i] < 0) continue;`, ¿cuánto valdría V(G)? Razónalo sin dibujar el grafo completo.
4. Haz `push`.

/// details | Ampliación opcional: integración continua
Añade al repositorio el fichero `.github/workflows/pruebas.yml` del apartado 9 de la teoría y comprueba en la pestaña **Actions** que las pruebas se ejecutan solas tras cada `push`. Incluye una captura en la memoria. No suma nota, pero el profesor lo tendrá en cuenta para redondear.
///

---

## Entregables

1. **Memoria en PDF** (`apellido1_apellido2_nombre_ut03p02.pdf`) con:
    * Portada con tu nombre y el **enlace al repositorio**.
    * Fotos del cuaderno con la parte A de los dos métodos.
    * Una tabla resumen por método: camino, entrada, esperado y **nombre de la prueba JUnit** que lo implementa.
    * Respuesta razonada a la pregunta de la parte D.3.
    * **Captura 1:** ventana Run de IntelliJ con todas las pruebas en verde.
    * **Captura 2:** informe de cobertura con los porcentajes de `AnalisisArrays` (clases, métodos, líneas y ramas).
    * **Captura 3:** salida de `mvn test`.
    * **Captura 4:** historial de commits.
2. **Repositorio de GitHub** con el proyecto Maven completo.

## Entrega en Moodle

Sube **solo el PDF** a la tarea **"UT03 · PRÁCTICA 02"** del aula virtual antes de la fecha indicada en ella. Las prácticas entregadas fuera de plazo no se califican.

## Rúbrica

| Criterio | CE | Excelente | Adecuado | Insuficiente | Puntos |
| --- | --- | --- | --- | --- | --- |
| Grafos de flujo | b) | Los dos grafos correctos, con nodos predicado y nodo final únicos (2,5) | Un grafo correcto y otro con errores menores (1,25) | Grafos incorrectos o sin hacer (0) | 2,5 |
| Complejidad ciclomática | b) | Tres fórmulas coincidentes en los dos métodos (1,5) | Correcta con una o dos fórmulas (0,75) | Incorrecta (0) | 1,5 |
| Caminos y casos de prueba | b) | Caminos independientes y casos con entrada y esperado para todos (2) | Falta algún camino o algún esperado (1) | No identifica los caminos (0) | 2 |
| Pruebas JUnit por camino y valores límite | f) | Una prueba por camino con `@DisplayName`, más la prueba parametrizada de límites (2,5) | Faltan caminos o la prueba de límites (1,25) | Pruebas que no se corresponden con los caminos (0) | 2,5 |
| Cobertura y Maven | g) | 100 % de líneas y ramas, `mvn test` y respuesta D.3 correcta (1) | Cobertura incompleta o respuesta D.3 sin razonar (0,5) | Sin cobertura ni Maven (0) | 1 |
| Git y memoria | — | Commits descriptivos por parte, memoria completa (0,5) | Pocos commits o faltan capturas (0,25) | Sin repositorio (0) | 0,5 |
| | | | | | **10** |
