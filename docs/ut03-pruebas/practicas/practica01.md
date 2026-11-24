# PRÁCTICA 01: CAJA NEGRA Y JUNIT — COSTE DE ENVÍO

---

| | |
| --- | --- |
| **Resultado de aprendizaje** | RA3. Verifica el funcionamiento de programas diseñando y realizando pruebas. |
| **Criterios de evaluación** | RA3 – CE b) Se han definido casos de prueba · RA3 – CE f) Se han efectuado pruebas unitarias de clases y funciones · RA3 – CE g) Se han implementado pruebas automáticas |
| **Duración** | 2 sesiones en el aula + 1 h en casa |
| **Modalidad** | Individual |

## Objetivos

* Diseñar casos de prueba con **clases de equivalencia** y **valores límite** a partir de una especificación.
* Convertir los casos de prueba en **pruebas JUnit 5** parametrizadas y de excepciones.
* Ejecutar las pruebas de forma **automática con Maven**.
* Usar las pruebas para **encontrar defectos** reales y llevar el trabajo con **Git**.

## Requisitos

* IntelliJ IDEA con JDK 21.
* Haber hecho los ejercicios de clase EC2, EC4 y EC5.
* Cuenta de GitHub.

---

## Enunciado

Una empresa de mensajería te pasa la clase `CalculadoraEnvio`, que calcula el coste de enviar un paquete. Esta es la **especificación**:

* `double calcularCoste(double pesoKg, String zona)`
* El peso debe ser **mayor que 0 y como máximo 30 kg**. Si no, lanza `IllegalArgumentException`.
* La zona debe ser exactamente `"PENINSULA"`, `"BALEARES"` o `"CANARIAS"`. Cualquier otro valor, incluido `null`, lanza `IllegalArgumentException`.
* Precio base según el peso:

| Peso | Precio base |
| --- | --- |
| Hasta 2 kg (incluido) | 4,50 € |
| Más de 2 kg y hasta 10 kg (incluido) | 7,90 € |
| Más de 10 kg y hasta 30 kg (incluido) | 12,50 € |

* Recargo por zona: península, 0 €; Baleares, **+3,00 €**; Canarias, **+8,00 €**.

Los pesos se expresan con dos decimales como máximo, así que el valor "inmediatamente posterior" a un límite está a **0,01 kg**.

/// warning | La clase tiene defectos
El programador asegura que la clase "funciona", pero contiene **dos defectos**. Tu batería de pruebas debe encontrarlos. **No leas el código buscando los errores**: diseña primero las pruebas a partir de la especificación, como harías en una empresa con una caja negra.
///

### Parte A. Diseño de los casos de prueba (en la memoria)

1. **Clases de equivalencia** para el peso y para la zona: tabla con número de clase, tipo (válida / no válida), condición, representante y resultado esperado.
2. **Valores límite** del peso: para cada límite (0, 2, 10 y 30), el valor del límite y los inmediatamente anterior y posterior, con su resultado esperado.
3. **Tabla de casos de prueba** con la plantilla de la teoría (ID, descripción, entrada, resultado esperado). Combina peso y zona: no hace falta probar todos los pesos en todas las zonas, pero **cada zona** debe aparecer con **cada tramo** de peso al menos una vez.

### Parte B. Proyecto Maven y repositorio

1. Crea un repositorio **privado** en GitHub llamado `eedd-ut03-practica01` e invita a tu profesor como colaborador.
2. Crea en IntelliJ un proyecto Maven `ut03-practica01` con el `pom.xml` de la teoría (apartado 7.2). Paquete: `es.albares.eedd.ut03`.
3. Añade la clase tal como te la dan:

```java
package es.albares.eedd.ut03;

/**
 * Calcula el coste de envío de un paquete según su peso y la zona de destino.
 */
public class CalculadoraEnvio {

    public static final String PENINSULA = "PENINSULA";
    public static final String BALEARES = "BALEARES";
    public static final String CANARIAS = "CANARIAS";

    public static double calcularCoste(double pesoKg, String zona) {
        if (pesoKg <= 0 || pesoKg >= 30) {
            throw new IllegalArgumentException("Peso no válido: " + pesoKg);
        }
        if (zona == null) {
            throw new IllegalArgumentException("La zona no puede ser nula");
        }
        double coste;
        if (pesoKg < 2) {
            coste = 4.50;
        } else if (pesoKg <= 10) {
            coste = 7.90;
        } else {
            coste = 12.50;
        }
        switch (zona) {
            case PENINSULA:
                break;
            case BALEARES:
                coste += 3.00;
                break;
            case CANARIAS:
                coste += 8.00;
                break;
            default:
                throw new IllegalArgumentException("Zona desconocida: " + zona);
        }
        return coste;
    }
}
```

4. Añade un `.gitignore` con `target/` y `.idea/`, haz el primer commit (`Proyecto inicial con CalculadoraEnvio`) y súbelo.

### Parte C. Pruebas JUnit

Crea `CalculadoraEnvioTest` en `src/test/java` e implementa **todos** los casos de tu tabla de la parte A:

1. Una prueba `@ParameterizedTest` con `@CsvSource` para los **tramos de peso en península** (clases válidas y valores límite).
2. Una prueba `@ParameterizedTest` con `@CsvSource` para los **recargos por zona** (columnas: peso, zona, esperado).
3. Una prueba `@ParameterizedTest` con `@ValueSource(doubles = …)` para los **pesos no válidos**.
4. Una prueba `@ParameterizedTest` con `@ValueSource(strings = …)` para las **zonas no válidas**, y una prueba `@Test` aparte para la zona `null`.
5. Todas con `@DisplayName` o con un `name` legible en la prueba parametrizada.
6. Recuerda usar un margen al comparar `double`: `assertEquals(esperado, obtenido, 0.001)`.

Haz commit: `Pruebas de caja negra de CalculadoraEnvio`.

### Parte D. Ejecución automática y corrección

1. Ejecuta `mvn test` en la terminal. Algunas pruebas fallarán: **anota en la memoria** qué ejecuciones fallan y el mensaje de cada una.
2. A partir de los fallos, localiza los **dos defectos** en el código. Explica cada uno en la memoria: línea, qué dice el código, qué dice la especificación.
3. Corrige **cada defecto en un commit distinto**, con un mensaje que lo describa (por ejemplo `Corrige el límite superior del peso (30 kg es válido)`).
4. Vuelve a ejecutar `mvn test`: todas las pruebas deben pasar. Ejecuta también la clase de prueba **con cobertura**.
5. Haz `push`.

---

## Entregables

1. **Memoria en PDF** (`apellido1_apellido2_nombre_ut03p01.pdf`) con:
    * Portada con tu nombre y el **enlace al repositorio**.
    * Parte A: las tres tablas.
    * Parte D: lista de ejecuciones que fallan con su mensaje, explicación de los dos defectos y su corrección.
    * **Captura 1:** salida de `mvn test` con fallos (antes de corregir).
    * **Captura 2:** salida de `mvn test` con `BUILD SUCCESS` (después de corregir).
    * **Captura 3:** informe de cobertura de `CalculadoraEnvio`.
    * **Captura 4:** historial de commits (`git log --oneline` o la pestaña *Commits* de GitHub).
2. **Repositorio de GitHub** con el proyecto Maven completo (sin la carpeta `target/`).

## Entrega en Moodle

Sube **solo el PDF** a la tarea **"UT03 · PRÁCTICA 01"** del aula virtual antes de la fecha indicada en ella. El profesor revisará el repositorio a partir del enlace. Las prácticas entregadas fuera de plazo no se califican (programación, apartado 9.3).

## Rúbrica

| Criterio | CE | Excelente | Adecuado | Insuficiente | Puntos |
| --- | --- | --- | --- | --- | --- |
| Clases de equivalencia | b) | Todas las clases de peso y zona, válidas y no válidas, con representante y esperado (2) | Falta alguna clase o algún esperado (1) | Faltan varias clases o están mal planteadas (0) | 2 |
| Valores límite y tabla de casos | b) | Los cuatro límites con anterior/posterior y tabla completa (2) | Falta algún límite o la tabla está incompleta (1) | No aplica la técnica (0) | 2 |
| Pruebas JUnit | f) | Todas las pruebas pedidas, parametrizadas, con excepciones, `null` y nombres legibles (3) | Faltan algunos casos o no usa pruebas parametrizadas (1,5) | Menos de la mitad de los casos o no compilan (0) | 3 |
| Detección y corrección de defectos | b), f) | Detecta, explica y corrige los dos defectos (1,5) | Detecta y corrige uno (0,75) | No los detecta con sus pruebas (0) | 1,5 |
| Ejecución automática con Maven | g) | `mvn test` antes y después, con cobertura (1) | Solo una de las ejecuciones (0,5) | No usa Maven (0) | 1 |
| Git y memoria | — | Commits separados y descriptivos, memoria clara con todas las capturas (0,5) | Pocos commits o faltan capturas (0,25) | Sin repositorio o memoria desordenada (0) | 0,5 |
| | | | | | **10** |
