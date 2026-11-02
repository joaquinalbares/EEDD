# Práctica UT03P01: Instalación de Eclipse IDE, Gestión con Maven y Depuración con JUnit 5 en Xubuntu

/// admonition | Verificación del Entorno

    Asegúrate de tener instalado el Kit de Desarrollo de Java (JDK) en tu sistema Xubuntu antes de iniciar Eclipse:
/// 


```bash title="Terminal"
java --version
echo $JAVA_HOME
sudo apt update

```

---

## 🛠️ Desarrollo Paso a Paso

### Paso 1: Instalación de Eclipse IDE mediante Tarball (`.tar.gz`)

1. **Descargar la versión oficial de Eclipse:**
Abre el navegador o utiliza la terminal para descargar el paquete comprimido de *Eclipse IDE for Java Developers*:
```bash title="Terminal - Descarga de Eclipse"
cd ~/Descargas
wget -c "https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2024-03/R/eclipse-java-2024-03-R-linux-gtk-x86_64.tar.gz&r=1" -O eclipse-java.tar.gz

```


2. **Extraer el archivo en el directorio personal o `/opt`:**
```bash title="Terminal - Extracción"
tar -xvf eclipse-java.tar.gz -C ~/

```


3. **Lanzar Eclipse IDE:**
Navega al directorio extraído y ejecuta el binario:
```bash title="Terminal - Ejecución"
~/eclipse/eclipse &

```


4. **Seleccionar el Espacio de Trabajo (*Workspace*):**
Al iniciar, selecciona el directorio por defecto `/home/usuario/eclipse-workspace` y haz clic en **Launch**.

---

### Paso 2: Creación de un Proyecto Java con Apache Maven

1. **Crear nuevo proyecto Maven:**
* En el menú superior de Eclipse, selecciona **File ➔ New ➔ Maven Project**.
* Marca la casilla **Create a simple project (skip archetype selection)** y haz clic en **Next**.


2. **Definir las coordenadas de Maven:**
* **Group Id:** `es.iesalbares.ed`
* **Artifact Id:** `calculadora-maven`
* **Version:** `1.0.0-SNAPSHOT`
* Haz clic en **Finish**.


3. **Configurar las dependencias de JUnit 5 (`pom.xml`):**
Abre el archivo `pom.xml` generado en la raíz del proyecto y añade la dependencia de JUnit 5 dentro de la etiqueta `<dependencies>`:
```xml title="pom.xml"
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>es.iesalbares.ed</groupId>
  <artifactId>calculadora-maven</artifactId>
  <version>1.0.0-SNAPSHOT</version>

  <properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <!-- Dependencia oficial de JUnit 5 (Jupiter) -->
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter</artifactId>
      <version>5.10.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>

```


*Guarda el archivo (++ctrl++ + ++s++). Maven descargará automáticamente las librerías necesarias.*

---

### Paso 3: Desarrollo de la Lógica Orientada a Objetos

1. Despliega la carpeta `src/main/java`.
2. Haz clic derecho sobre `src/main/java` ➔ **New ➔ Class**.
* **Package:** `es.iesalbares.ed.modelo`
* **Name:** `Calculadora`


3. Escribe el siguiente código:

```java title="src/main/java/es/iesalbares/ed/modelo/Calculadora.java"
package es.iesalbares.ed.modelo;

public class Calculadora {

    public double sumar(double a, double b) {
        return a + b;
    }

    public double dividir(double a, double b) {
        if (b == 0) {
            throw new IllegalArgumentException("No se puede dividir por cero");
        }
        return a / b;
    }

    public int calcularFactorial(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("El número debe ser no negativo");
        }
        int resultado = 1;
        for (int i = 1; i <= n; i++) {
            resultado *= i; // Punto clave para inspección en depuración
        }
        return resultado;
    }
}

```

---

### Paso 4: Implementación de Pruebas Unitarias con JUnit 5

1. Despliega la carpeta `src/test/java`.
2. Clic derecho sobre `src/test/java` ➔ **New ➔ JUnit Test Case**.
* Selecciona **New JUnit Jupiter test (JUnit 5)**.
* **Package:** `es.iesalbares.ed.modelo`
* **Name:** `CalculadoraTest`
* **Class under test:** `es.iesalbares.ed.modelo.Calculadora`


3. Implementa los casos de prueba:

```java title="src/test/java/es/iesalbares/ed/modelo/CalculadoraTest.java"
package es.iesalbares.ed.modelo;

import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

class CalculadoraTest {

    private Calculadora calc;

    @BeforeEach
    void setUp() {
        calc = new Calculadora();
    }

    @Test
    @DisplayName("Suma exitosa de dos números")
    void testSumar() {
        assertEquals(5.0, calc.sumar(2.0, 3.0), "2 + 3 debe ser 5");
    }

    @Test
    @DisplayName("Excepción al dividir por cero")
    void testDividirPorCero() {
        assertThrows(IllegalArgumentException.class, () -> calc.dividir(10.0, 0.0));
    }

    @Test
    @DisplayName("Cálculo del factorial de 5")
    void testFactorial() {
        assertEquals(120, calc.calcularFactorial(5), "Factorial de 5 debe ser 120");
    }
}

```

4. **Ejecutar el Test:**
* Haz clic derecho sobre `CalculadoraTest.java` ➔ **Run As ➔ JUnit Test**.
* Comprueba que se abre la vista **JUnit** con la barra verde que confirma la superación de todas las pruebas.



---

### Paso 5: Depuración paso a paso e Inspección de Variables

1. **Establecer un Punto de Ruptura (*Breakpoint*):**
* Abre `Calculadora.java`.
* Haz doble clic sobre el margen izquierdo (junto al número de línea) en la instrucción dentro del bucle del factorial:
```java
resultado *= i;

```


* Aparecerá una bola azul indicando que el punto de ruptura está activo.


2. **Iniciar la sesión de depuración:**
* Abre `CalculadoraTest.java`.
* Haz clic derecho ➔ **Debug As ➔ JUnit Test**.
* Eclipse solicitará cambiar a la **Perspectiva de Depuración (Debug Perspective)**. Haz clic en **Switch**.


3. **Inspección de variables y control de flujo:**
* Observa la vista **Variables** (esquina superior derecha). Identifica los valores en tiempo real de `i`, `n` y `resultado`.
* Utiliza las teclas de control de depuración para avanzar en la ejecución:
* ++f5++ (**Step Into**): Entra dentro del método llamado.
* ++f6++ (**Step Over**): Ejecuta la línea actual y pasa a la siguiente.
* ++f8++ (**Resume**): Continúa la ejecución hasta el siguiente punto de ruptura o final del programa.


* Haz avanzar el bucle iterativamente con ++f6++ u ++f8++ observando cómo cambia el valor de la variable `resultado` en cada paso.



---

## 📑 Entregables de la Práctica

/// admonition | "Lista de Comprobación para la Entrega"

  Sube a la plataforma del curso un documento en PDF con la memoria de la práctica que incluya las siguientes capturas de pantalla:

  ```
  - [ ] **Captura 1:** Ventana principal de Eclipse mostrando la estructura del proyecto Maven en el **Package Explorer** y el archivo `pom.xml` con las dependencias.
  - [ ] **Captura 2:** Resultado de la ejecución de las pruebas unitarias mostrando el panel con la **barra verde de JUnit**.
  - [ ] **Captura 3:** Pantalla completa en la **Perspectiva de Depuración**, donde se aprecie claramente la línea detenida en el punto de ruptura y el panel **Variables** con los valores asignados en tiempo de ejecución.
  ```
///
