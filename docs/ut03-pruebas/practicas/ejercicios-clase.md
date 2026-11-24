# UT 03: EJERCICIOS DE CLASE

---

/// note | Cómo se evalúan estos ejercicios
* Las **actividades de cuaderno (AC)** se hacen a mano en tu cuaderno y cuentan para el **10 % de cuaderno**.
* Los **ejercicios de clase (EC)** se hacen en el aula, de forma guiada, y cuentan para el **10 % de ejercicios de clase**. Se corrigen en clase; enseña al profesor el resultado (cuaderno, pantalla o repositorio) antes de que termine la sesión.

Todos los proyectos Java de la unidad usan **IntelliJ IDEA**, **JDK 21**, **Maven** y **JUnit 5**. El paquete de todos los ejercicios es `es.albares.eedd.ut03`.
///

---

## AC1. Clasifica las pruebas

**RA3 – CE a)** · Duración: 20 min · En el cuaderno

Para cada situación, indica: **nivel** (unitaria, integración, sistema o aceptación), **técnica** (caja negra o caja blanca) y si es **manual o automática**. Si además encaja en otro tipo (regresión, rendimiento, seguridad, usabilidad, humo…), indícalo.

1. Escribes una prueba JUnit que comprueba que `calcularIVA(100)` devuelve `21.0`.
2. El cliente usa la aplicación de reservas durante una semana con sus empleados antes de pagarla.
3. Dibujas el grafo de flujo de un método y diseñas un caso para cada camino independiente.
4. Simulas 5.000 usuarios conectados a la vez para ver si la web sigue respondiendo en menos de 2 segundos.
5. Tras corregir un error en el cálculo de descuentos, vuelves a ejecutar las 300 pruebas del proyecto.
6. Compruebas que el módulo de pedidos guarda correctamente los datos en la base de datos MySQL.
7. Una compañera prueba el formulario de registro escribiendo `' OR 1=1 --` en el campo de usuario.
8. Antes de probar a fondo una nueva versión, compruebas que arranca y que se puede iniciar sesión.
9. Observas a cinco personas mayores intentando comprar una entrada en la web y anotas dónde se atascan.
10. Un *workflow* de GitHub Actions ejecuta `mvn test` cada vez que alguien hace `push`.

**Criterios de corrección:** 1 punto por situación bien clasificada (nivel, técnica y modo). Se acepta cualquier respuesta razonada si la situación admite varias.

---

## AC2. Herramientas de prueba y depuración de tu IDE

**RA3 – CE c)** · Duración: 20 min · En el cuaderno, con IntelliJ abierto

Localiza en IntelliJ cada herramienta y completa en tu cuaderno una tabla con tres columnas: **herramienta**, **dónde está / cómo se abre** y **para qué sirve**.

* Ejecutar una clase de prueba y ver el informe de resultados.
* Crear una clase de prueba a partir de una clase.
* Ejecutar las pruebas con cobertura.
* Ventana Maven y fase `test`.
* Iniciar la depuración de un programa y de una prueba.
* Poner un punto de ruptura de línea y uno condicional.
* Ver todos los puntos de ruptura (*View Breakpoints*).
* Step Over, Step Into, Step Out, Run to Cursor y Resume (con sus teclas).
* Paneles Variables, Watches y Frames.
* Evaluate Expression y Set Value.

Termina con una línea: ¿qué **dos** herramientas tiene también VS Code (con el *Extension Pack for Java*) y cómo se llaman allí?

**Criterios de corrección:** tabla completa (6 puntos), teclas correctas (2 puntos), comparación con VS Code (2 puntos).

---

## EC2. Caja negra: tarifa del aparcamiento

**RA3 – CE b)** · Duración: 45 min · En el cuaderno

Un aparcamiento calcula su tarifa con el método `int calcularTarifa(int minutos)`:

* De 0 a 15 minutos: **gratis**.
* De 16 a 60 minutos: **2 €**.
* Más de 60 minutos: **2 €** más **1 € por cada hora o fracción** adicional (61 minutos son 3 €; 121 minutos, 4 €).
* La tarifa nunca supera **15 €** al día.
* Valores válidos: de 0 a 1440 minutos (un día). Fuera de ese rango lanza `IllegalArgumentException`.

Se pide:

1. Tabla de **clases de equivalencia** (válidas y no válidas) con un representante de cada una y su resultado esperado.
2. Tabla de **valores límite**. Pista: ¿a partir de qué minuto se alcanza el tope de 15 €?
3. Tabla final de **casos de prueba** con la plantilla de la teoría (ID, descripción, entrada, resultado esperado). Mínimo 12 casos.

**Criterios de corrección:** clases de equivalencia (3 puntos), valores límite incluido el del tope (4 puntos), tabla de casos con formato correcto (3 puntos).

---

## EC3. Caja blanca: camino básico

**RA3 – CE b)** · Duración: 55 min · En el cuaderno

Para **cada uno** de estos métodos: copia el código y numera los nodos, dibuja el grafo de flujo, calcula V(G) con **las tres fórmulas**, escribe los caminos independientes y diseña un caso de prueba para cada camino.

**Método A**

```java
public static int sumarDigitos(int n) {
    if (n < 0) {
        n = -n;
    }
    int suma = 0;
    while (n > 0) {
        suma += n % 10;
        n /= 10;
    }
    return suma;
}
```

**Método B**

```java
public static boolean esBisiesto(int anio) {
    boolean bisiesto;
    if ((anio % 4 == 0 && anio % 100 != 0) || anio % 400 == 0) {
        bisiesto = true;
    } else {
        bisiesto = false;
    }
    return bisiesto;
}
```

En el método B, ¿hay algún camino del grafo que **no se pueda recorrer** con ningún año? Explica por qué.

**Criterios de corrección (por método, 5 puntos):** numeración y grafo (2), V(G) con las tres fórmulas coincidentes (1), caminos independientes (1), casos de prueba con resultado esperado (1).

---

## EC4. Primer proyecto Maven con JUnit 5

**RA3 – CE f), g)** · Duración: 2 sesiones · En el ordenador, con Git

1. Crea en IntelliJ un proyecto **Maven** llamado `ut03-ejercicios` (GroupId `es.albares.eedd`, JDK 21), siguiendo el apartado 7.4 de la teoría.
2. Sustituye el `pom.xml` por el de la teoría (apartado 7.2) y recarga Maven.
3. Inicializa Git en la carpeta del proyecto y haz el primer commit: `git init`, `git add .`, `git commit -m "Proyecto Maven inicial"`. Añade un `.gitignore` con la línea `target/`.
4. Crea la clase `Calculadora` en `src/main/java/es/albares/eedd/ut03`:

    ```java
    package es.albares.eedd.ut03;

    public class Calculadora {

        public int sumar(int a, int b) {
            return a + b;
        }

        public int restar(int a, int b) {
            return a - b;
        }

        public int multiplicar(int a, int b) {
            return a * b;
        }

        public int dividir(int a, int b) {
            if (b == 0) {
                throw new ArithmeticException("No se puede dividir entre cero");
            }
            return a / b;
        }

        public boolean esPar(int n) {
            return n % 2 == 0;
        }
    }
    ```

5. Genera `CalculadoraTest` con ++ctrl+shift+t++. Crea el objeto `Calculadora` en un método `@BeforeEach`.
6. Escribe **al menos 8 pruebas**: dos para `sumar` (con positivos y con un negativo), una para `restar`, una para `multiplicar` por cero, dos para `dividir` (exacta y con decimales truncados), una que compruebe la excepción **y su mensaje** con `assertThrows`, y una para `esPar` con `assertAll` que compruebe 0, un par, un impar y dos negativos.
7. Ejecútalas desde el IDE. Después ciérralo todo y ejecútalas desde la terminal con `mvn test`. Localiza en la salida la línea `Tests run: …, Failures: …`.
8. Rompe a propósito `sumar` (`return a - b;`), ejecuta `mvn package` y observa qué ocurre. Deshaz el cambio.
9. Commit: `git commit -am "Pruebas unitarias de Calculadora"`.

**Criterios de corrección:** proyecto Maven correcto y en Git (2 puntos), 8 pruebas que pasan (5 puntos), ejecución con `mvn test` (2 puntos), explicación oral de qué pasó en el paso 8 (1 punto).

---

## EC5. Pruebas parametrizadas: el cine

**RA3 – CE b), f)** · Duración: 45 min · En el ordenador, mismo proyecto

1. Copia en el proyecto la clase `PrecioEntrada` del apartado 5.3 de la teoría (escríbela tú a partir de la especificación).
2. Crea `PrecioEntradaTest` con:
    * Una prueba `@ParameterizedTest` con `@CsvSource` que cubra **todas** las clases válidas y **todos** los valores límite válidos de las tablas de la teoría.
    * Una prueba `@ParameterizedTest` con `@ValueSource` para las edades no válidas.
    * Un nombre legible en cada ejecución con `name = "edad {0} → {1} €"`.
3. Ejecuta la clase de prueba **con cobertura**. ¿Qué porcentaje de líneas cubre? ¿Hay alguna línea amarilla o roja?
4. Introduce un error de límite en la clase (cambia `edad <= 17` por `edad < 17`) y comprueba qué prueba lo detecta. Deshaz el cambio.
5. Commit.

**Criterios de corrección:** pruebas parametrizadas correctas (6 puntos), 100 % de cobertura de líneas (2 puntos), identifica la prueba que detecta el error (2 puntos).

---

## EC6. Depuración guiada: contar palabras

**RA3 – CE d), e)** · Duración: 2 sesiones · En el ordenador y en el cuaderno

Este método debería contar las palabras de una frase (separadas por uno o varios espacios), pero tiene un defecto:

```java
public static int contarPalabras(String frase) {
    int palabras = 0;
    boolean dentroPalabra = false;
    for (int i = 0; i < frase.length(); i++) {
        char c = frase.charAt(i);
        if (c != ' ') {
            if (!dentroPalabra) {
                palabras++;
                dentroPalabra = true;
            }
        }
    }
    return palabras;
}
```

1. Añádelo a una clase `Texto` y crea un `main` que imprima `contarPalabras("hola que tal")`. ¿Qué devuelve? ¿Qué debería devolver?
2. **Punto de ruptura de línea** en `if (c != ' ') {`. Lanza en modo depuración y avanza con ++f9++. En el cuaderno, haz una **traza**: tabla con `i`, `c`, `dentroPalabra` y `palabras` en cada parada.
3. Añade una **watch** con la expresión `c == ' '`.
4. Cambia el punto de ruptura a **condicional** con la condición `c == ' '`. ¿En qué valores de `i` se detiene? ¿Qué valor tiene `dentroPalabra` en esas paradas? ¿Qué debería valer después del espacio?
5. **Modifica en ejecución:** en la parada de `i == 4`, usa **Set Value** (++f2++ en el panel Variables) para poner `dentroPalabra` a `false` y continúa con ++f9++. Haz lo mismo en la parada de `i == 8`. ¿Cuántas palabras devuelve ahora? ¿Qué te confirma eso?
6. Usa **Evaluate Expression** (++alt+f8++) para calcular `frase.split(" ").length` y compáralo con el resultado. ¿Funcionaría `split` con dos espacios seguidos?
7. Corrige el defecto y comprueba `"hola que tal"`, `"  dos   espacios "`, `""` y `"una"`.

**Criterios de corrección:** traza correcta en el cuaderno (3 puntos), uso del punto de ruptura condicional y la watch (3 puntos), uso de Set Value y Evaluate Expression con conclusión (2 puntos), corrección del defecto (2 puntos).

---

## EC7. Documenta la incidencia

**RA3 – CE h)** · Duración: 40 min · En el ordenador, con GitHub

1. Sube el proyecto `ut03-ejercicios` a un repositorio de GitHub (si no lo has hecho ya).
2. **Antes de corregir** el defecto de EC6 (si ya lo corregiste, vuelve a la versión anterior con `git stash` o deshaciendo el cambio), escribe una prueba JUnit `contarPalabrasConEspacios` que lo reproduzca. Comprueba que **falla**.
3. Crea una **Issue** en GitHub con la plantilla de informe de incidencia de la teoría (apartado 11): título, entorno, pasos para reproducirla, resultado esperado, resultado obtenido, gravedad, prioridad y, como evidencia, el mensaje de la prueba que falla.
4. Corrige el defecto y comprueba que la prueba pasa.
5. Haz commit con el mensaje `Corrige el conteo de palabras tras un espacio. Fixes #N` (N = número de tu issue) y haz `push`. Comprueba que la issue se ha cerrado sola.

**Criterios de corrección:** prueba que reproduce el defecto (3 puntos), informe completo y reproducible (5 puntos), cierre de la issue desde el commit (2 puntos).
