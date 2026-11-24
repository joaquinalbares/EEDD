# PRÁCTICA 03: DEPURACIÓN CON INTELLIJ — ESTADÍSTICAS DE NOTAS

---

| | |
| --- | --- |
| **Resultado de aprendizaje** | RA3. Verifica el funcionamiento de programas diseñando y realizando pruebas. |
| **Criterios de evaluación** | RA3 – CE d) Se han utilizado herramientas de depuración para definir puntos de ruptura y seguimiento · RA3 – CE e) Se han utilizado las herramientas de depuración para examinar y modificar el comportamiento de un programa en tiempo de ejecución |
| **Duración** | 1 sesión en el aula + 2 h en casa |
| **Modalidad** | Individual |

## Objetivos

* Usar **puntos de ruptura** de línea, condicionales y de registro.
* Seguir la ejecución **paso a paso** (Step Over, Step Into, Step Out, Run to Cursor).
* **Examinar** el estado del programa con Variables, Watches, Frames y Evaluate Expression.
* **Modificar** el comportamiento del programa en ejecución con Set Value.
* Registrar el proceso de depuración en una **ficha** y corregir los defectos con **Git**.

## Requisitos

* IntelliJ IDEA con JDK 21 y Git.
* Haber hecho EC6 y la actividad de cuaderno AC2.

---

## Enunciado

El departamento quiere un programa que saque estadísticas de las notas de un grupo, pero los resultados no cuadran con los que ha calculado a mano el tutor.

### Código de partida

Crea un proyecto Maven `ut03-practica03` (paquete `es.albares.eedd.ut03`), un repositorio privado `eedd-ut03-practica03` en GitHub (invita al profesor) y añade estas dos clases **sin modificarlas**. Primer commit: `Código de partida`.

```java
package es.albares.eedd.ut03;

/**
 * Estadísticas básicas sobre las notas de un grupo.
 */
public class EstadisticasNotas {

    // Devuelve la media aritmética de las notas
    public static double media(double[] notas) {
        double suma = 0;
        for (int i = 1; i < notas.length; i++) {
            suma += notas[i];
        }
        return suma / notas.length;
    }

    // Devuelve cuántas notas son suspensos (nota menor que 5)
    public static int contarSuspensos(double[] notas) {
        int suspensos = 0;
        for (double nota : notas) {
            if (nota <= 5) {
                suspensos++;
            }
        }
        return suspensos;
    }

    // Devuelve la posición (índice) de la nota más alta
    public static int posicionMaxima(double[] notas) {
        int pos = 0;
        for (int i = 1; i < notas.length; i++) {
            if (notas[i] > notas[pos]) {
                pos = i;
            }
        }
        return pos;
    }

    // Devuelve la calificación en texto: Insuficiente (<5), Suficiente [5,6),
    // Bien [6,7), Notable [7,9) y Sobresaliente [9,10]
    public static String calificacion(double nota) {
        if (nota < 5) {
            return "Insuficiente";
        } else if (nota < 6) {
            return "Suficiente";
        } else if (nota < 7) {
            return "Bien";
        } else if (nota <= 9) {
            return "Notable";
        } else {
            return "Sobresaliente";
        }
    }
}
```

```java
package es.albares.eedd.ut03;

public class Main {

    public static void main(String[] args) {
        double[] notas = {6.5, 7.0, 4.25, 8.0, 3.5, 9.0, 5.5, 6.0, 7.75, 2.0,
                          5.0, 8.5, 6.25, 4.5, 7.0, 5.75, 6.0, 8.25, 3.0, 7.5};

        double media = EstadisticasNotas.media(notas);
        int suspensos = EstadisticasNotas.contarSuspensos(notas);
        int posMax = EstadisticasNotas.posicionMaxima(notas);
        String calificacionMejor = EstadisticasNotas.calificacion(notas[posMax]);

        System.out.println("Media del grupo:          " + media + "  (esperado: 6.0625)");
        System.out.println("Número de suspensos:      " + suspensos + "  (esperado: 5)");
        System.out.println("Posición de la mejor:     " + posMax + "  (esperado: 5)");
        System.out.println("Calificación de la mejor: " + calificacionMejor + "  (esperado: Sobresaliente)");
    }
}
```

/// warning | Normas de la práctica
* **No modifiques el código** hasta la parte D. Las partes B y C se hacen solo con el depurador.
* No añadas `System.out.println` para buscar los errores: usa las herramientas del IDE.
* Cada captura debe mostrar el **editor** (con el punto de ruptura visible) y la **ventana Debug**.
///

### Parte A. Síntomas

Ejecuta `Main` y anota en la ficha qué resultados **no coinciden** con los esperados. Hay **tres defectos**, uno en cada uno de los métodos `media`, `contarSuspensos` y `calificacion`.

### Parte B. Localiza los defectos con el depurador (CE d)

**Defecto 1 · `media`: punto de ruptura de línea y paso a paso**

1. Pon un punto de ruptura de línea en `suma += notas[i];` y lanza `Main` en modo depuración (++shift+f9++).
2. Cuando se detenga, observa en el panel **Variables** el valor de `i`. Compáralo con el índice que debería tener en la primera vuelta.
3. Añade una **watch** con la expresión `notas[i]` y otra con `suma`. Avanza tres vueltas con ++f9++ y anota sus valores.
4. Quita el punto de ruptura, usa **Step Out** (++shift+f8++) para volver a `main`, avanza una línea con ++f8++ y comprueba en **Variables** el valor que ha recibido la variable `media`.
5. **Captura B1.**

**Defecto 2 · `contarSuspensos`: punto de ruptura de registro y condicional**

1. Pon un punto de ruptura en `suspensos++;`. Haz clic derecho sobre él, **desmarca *Suspend*** y marca **Evaluate and log** con la expresión `"Cuento como suspenso: " + nota`.
2. Ejecuta en modo depuración. La consola listará todas las notas que el método cuenta como suspenso **sin parar el programa**. ¿Cuál sobra?
3. Ahora vuelve a marcar *Suspend* y escribe una **condición** para que se detenga **solo** cuando va a contar una nota que no es suspenso. Ejecuta y comprueba que para una sola vez.
4. En esa parada, abre **Evaluate Expression** (++alt+f8++) y evalúa `nota < 5` y `nota <= 5`. ¿Qué te dicen?
5. **Captura B2** (consola con el registro) y **captura B3** (parada condicional con la evaluación).

**Defecto 3 · `calificacion`: Step Into y Frames**

1. Pon un punto de ruptura en la línea de `main` que llama a `calificacion`.
2. Cuando se detenga, entra en el método con **Step Into** (++f7++). Si IntelliJ te deja elegir entre varias llamadas de la línea, usa **Smart Step Into** (++shift+f7++).
3. Mira el panel **Frames**: ¿qué métodos hay en la pila? Haz clic en `main` y comprueba que puedes ver `posMax` y `notas` desde ahí. Vuelve a `calificacion`.
4. Avanza con **Step Over** (++f8++) y anota por qué rama sale el método con `nota = 9.0`.
5. **Captura B4.**

### Parte C. Examina y modifica en tiempo de ejecución (CE e)

1. **Evaluate Expression como banco de pruebas.** Con el programa detenido dentro de `calificacion`, evalúa `calificacion(8.99)`, `calificacion(9.0)`, `calificacion(9.01)` y `calificacion(10)`. Anota los resultados en una tabla y deduce dónde está exactamente el límite erróneo. **Captura C1.**
2. **Set Value sobre un parámetro.** Vuelve a detenerte al entrar en `calificacion`. Antes de que se evalúe ninguna condición, cambia con **Set Value** (++f2++) el valor de `nota` a `9.5` y continúa. ¿Qué calificación imprime ahora `main`? **Captura C2.**
3. **Set Value para corregir un resultado.** Detente en `return suma / notas.length;` del método `media`. Calcula con **Evaluate Expression** cuánto debería valer `suma` (pista: `suma + notas[0]`) y ponle ese valor con **Set Value**. Continúa: `main` debe imprimir la media correcta **sin haber cambiado el código**. **Captura C3.**
4. **Run to Cursor.** Pon el cursor en la línea `return pos;` de `posicionMaxima` y usa **Run to Cursor** (++alt+f9++) sin poner ningún punto de ruptura allí. Anota el valor de `pos` y de `notas[pos]`. ¿Tiene este método algún defecto?

### Parte D. Corrección

1. Corrige los tres defectos, **uno por commit**, con un mensaje que lo describa.
2. Escribe una clase `EstadisticasNotasTest` con **una prueba de regresión por defecto** (como mínimo), que habría fallado antes de la corrección y ahora pasa.
3. Ejecuta `Main` y `mvn test` y comprueba que todo cuadra. Haz `push`.

### Ficha de depuración

Para cada defecto, completa en la memoria esta tabla:

| Campo | Defecto 1 | Defecto 2 | Defecto 3 |
| --- | --- | --- | --- |
| Método y línea | | | |
| Síntoma observado | | | |
| Herramientas del depurador usadas | | | |
| Valores observados que revelan el defecto | | | |
| Causa | | | |
| Corrección aplicada (y commit) | | | |

---

## Entregables

1. **Memoria en PDF** (`apellido1_apellido2_nombre_ut03p03.pdf`) con portada y **enlace al repositorio**, la ficha de depuración, las respuestas de las partes B y C, y las capturas **B1, B2, B3, B4, C1, C2 y C3**, más una captura final de `mvn test` con todas las pruebas en verde.
2. **Repositorio de GitHub** con el código corregido, las pruebas y un commit por corrección.

## Entrega en Moodle

Sube **solo el PDF** a la tarea **"UT03 · PRÁCTICA 03"** del aula virtual antes de la fecha indicada en ella. Las prácticas entregadas fuera de plazo no se califican.

## Rúbrica

| Criterio | CE | Excelente | Adecuado | Insuficiente | Puntos |
| --- | --- | --- | --- | --- | --- |
| Síntomas (parte A) | — | Identifica los tres resultados erróneos (0,5) | Identifica dos (0,25) | Uno o ninguno (0) | 0,5 |
| Defecto 1: punto de ruptura de línea, watches y Step Out | d) | Uso correcto, valores anotados y causa explicada (2) | Uso correcto pero explicación incompleta (1) | No usa el depurador o no localiza el defecto (0) | 2 |
| Defecto 2: punto de registro y condicional | d) | Registro y condición correctos, una única parada, evaluación razonada (2) | Solo una de las dos técnicas (1) | No usa estas técnicas (0) | 2 |
| Defecto 3: Step Into y Frames | d) | Entra en el método, explica la pila y la rama recorrida (1,5) | Localiza el defecto sin explicar la pila (0,75) | No lo localiza con el depurador (0) | 1,5 |
| Examinar y modificar en ejecución (parte C) | e) | Las cuatro tareas con capturas y conclusiones correctas (2) | Dos o tres tareas (1) | Una o ninguna (0) | 2 |
| Corrección, regresión y Git (parte D) | — | Tres correcciones en commits separados y tres pruebas de regresión en verde (1,5) | Correcciones en un solo commit o faltan pruebas (0,75) | Sin corregir o sin repositorio (0) | 1,5 |
| Ficha de depuración y memoria | — | Ficha completa, clara y con todas las capturas (0,5) | Ficha incompleta (0,25) | Sin ficha (0) | 0,5 |
| | | | | | **10** |
