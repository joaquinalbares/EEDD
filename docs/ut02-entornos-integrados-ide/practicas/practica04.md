## UT02 - PRÁCTICA 04 · Instalación y configuración de IntelliJ IDEA para Java

| | |
|---|---|
| **Resultados de aprendizaje y criterios de evaluación** | RA2 – CE a), RA2 – CE b), RA2 – CE c), RA2 – CE d), RA2 – CE f), RA2 – CE g) |
| **Duración estimada** | 3 sesiones (3 h) |
| **Modalidad** | Individual |
| **Sistemas operativos** | Xubuntu 24.04 y Windows 11 |

---

### 🎯 Objetivos

- Instalar **IntelliJ IDEA** en Xubuntu y en Windows 11.
- Añadir y eliminar *plugins*.
- Configurar el JDK **Temurin 21** de la práctica 02 como SDK del proyecto.
- Crear, ejecutar y depurar un proyecto Java.
- Personalizar el entorno y automatizar tareas (acciones al guardar y plantillas de código).
- Configurar las actualizaciones.
- Comparar IntelliJ IDEA con VS Code generando el ejecutable del mismo código en los dos entornos.

### 🧰 Requisitos

- Haber terminado la **Práctica 02** (Temurin 21 con `JAVA_HOME` y `PATH`) y la **Práctica 03** (VS Code con el proyecto `hola-vscode`).
- Al menos **8 GB de RAM** (IntelliJ consume bastante más memoria que VS Code).
- Usuario con permisos de administrador y conexión a Internet.

/// note | ¿Y la «Community Edition»?
Hasta 2025, JetBrains publicaba dos productos: **IntelliJ IDEA Community Edition** (libre y gratuita) e **IntelliJ IDEA Ultimate** (de pago). Desde la versión **2025.3** se han unido en un único producto, **IntelliJ IDEA**:

- Todas las funciones de la antigua Community (Java, Kotlin, Maven, Gradle, Git, depurador, pruebas JUnit…) siguen siendo **gratuitas**, también para uso comercial, y **no necesitas crear una cuenta ni activar nada**.
- Las funciones avanzadas (antiguas de Ultimate) requieren suscripción. Al instalar, puede que te ofrezca una **prueba gratuita de 30 días**: **no la necesitas** para este módulo.

Si en algún tutorial lees «descarga IntelliJ Community», hoy significa descargar **IntelliJ IDEA** y usarlo en su modalidad gratuita.
///

/// admonition | Trabajo en el cuaderno
    type: tip

Durante la práctica, completa en tu **cuaderno** una tabla comparativa entre **VS Code** e **IntelliJ IDEA** con estas filas: tipo de herramienta (editor o IDE), licencia, forma de instalación en cada sistema, memoria que consume, cómo se añaden módulos, cómo se configura el JDK, cómo se ejecuta y depura, y cómo se actualiza. La usarás en el último apartado.
///

---

### 1. Instalación en Xubuntu 24.04

En Linux, la forma más sencilla y oficial de instalar IntelliJ IDEA es mediante **Snap**, el sistema de paquetes de Canonical. Los paquetes Snap se actualizan solos en segundo plano.

#### Paso 1: Comprobar que Snap está disponible

Xubuntu 24.04 trae Snap instalado. Compruébalo:

```bash
snap version
```

Si no aparece, instálalo:

```bash
sudo apt update && sudo apt install -y snapd
```

#### Paso 2: Instalar IntelliJ IDEA

```bash
sudo snap install intellij-idea --classic
```

/// details | ¿Qué significa `--classic`?
Los paquetes Snap se ejecutan normalmente **aislados** del resto del sistema. Un IDE necesita acceder a tus carpetas, al JDK, a Git…, así que se instala en modo **clásico**, sin ese aislamiento. Por eso Snap te pide confirmarlo con `--classic`.
///

#### Paso 3: Comprobar la instalación

```bash
snap list intellij-idea
snap info intellij-idea
```

En la salida de `snap info`, el apartado **commands** indica el nombre del comando que lanza IntelliJ desde la terminal. Pruébalo: debe abrirse el IDE. También encontrarás IntelliJ IDEA en el menú de aplicaciones de Xubuntu, dentro de **Desarrollo**.

/// details | Alternativa: instalar desde el archivo `.tar.gz`
Si no quieres usar Snap, descarga el `.tar.gz` para Linux desde [jetbrains.com/idea/download](https://www.jetbrains.com/idea/download/) y ejecuta:

```bash
sudo tar -xzf ideaIU-*.tar.gz -C /opt
/opt/idea-*/bin/idea.sh
```

En este caso tendrás que actualizarlo a mano y, si quieres el comando en la terminal, añadir la carpeta `bin` al `PATH` como en la práctica 02. También puedes usar **JetBrains Toolbox**, una aplicación que instala y actualiza todos los IDE de JetBrains.
///

---

### 2. Instalación en Windows 11

#### Paso 1: Descargar el instalador

Entra en [jetbrains.com/idea/download](https://www.jetbrains.com/idea/download/), selecciona **Windows** y descarga el instalador **`.exe`**.

#### Paso 2: Ejecutar el instalador

En la pantalla **Installation Options**, marca estas opciones:

| Opción | ¿Marcar? | Para qué sirve |
|---|---|---|
| **Create Desktop Shortcut** | Opcional | Acceso directo en el escritorio |
| **Update PATH Variable: Add "bin" folder to the PATH** | ✅ **Sí** | Poder lanzar IntelliJ desde la terminal |
| **Update Context Menu: Add "Open Folder as Project"** | ✅ Sí | Abrir una carpeta como proyecto con el botón derecho |
| **Create Associations: .java** | Opcional | Abrir los `.java` con doble clic |

Termina la instalación y **reinicia** el equipo si te lo pide: es necesario para que se aplique el cambio en el `PATH`.

#### Paso 3: Comprobar el `PATH`

En una **nueva** ventana de PowerShell:

```powershell
$env:Path -split ';' | Select-String "JetBrains"
```

Debe aparecer la carpeta `bin` de IntelliJ IDEA. Lanza el IDE desde la terminal con:

```powershell
idea64
```

---

### 3. Primer arranque

#### Paso 1: Pantalla de bienvenida

Al abrirlo por primera vez:

1. Acepta el **acuerdo de usuario**.
2. Decide si compartes estadísticas de uso anónimas (puedes rechazarlo).
3. Si te ofrece la prueba de las funciones avanzadas, **no la actives**: la versión gratuita es suficiente.

Llegarás a la pantalla **Welcome to IntelliJ IDEA**.

#### Paso 2: Tema e idioma

1. En la pantalla de bienvenida, abre **Customize**.
2. En **Color theme**, elige el tema que prefieras.
3. En **Language**, verás que IntelliJ solo ofrece paquetes oficiales de idioma para algunas lenguas, entre las que **no está el castellano**. Mantén el **inglés**: es el idioma en que encontrarás la documentación y la mayoría de tutoriales.

/// note | Nombres de los menús
Como el IDE está en inglés, en esta práctica los menús aparecen con su nombre original. En Windows y Linux, la configuración se abre en **File → Settings** (++ctrl+alt+s++).
///

---

### 4. *Plugins*: añadir y eliminar módulos

En IntelliJ, los módulos adicionales se llaman ***plugins***. A diferencia de VS Code, el soporte para Java ya viene **incluido de serie**.

#### Paso 1: Ver los *plugins* instalados

Abre **File → Settings → Plugins** y entra en la pestaña **Installed**. Localiza los *plugins* de Java, Maven, Git y JUnit que vienen de serie.

#### Paso 2: Instalar un *plugin*

1. Entra en la pestaña **Marketplace**.
2. Busca **Key Promoter X**. Este *plugin* te enseña los atajos de teclado: cada vez que usas el ratón para algo que tiene atajo, te lo muestra.
3. Pulsa **Install** y reinicia el IDE cuando te lo pida.

#### Paso 3: Desinstalar un *plugin*

Instala también el *plugin* **VSCode Keymap**, que permite usar en IntelliJ los mismos atajos de teclado que en VS Code. Después, en la pestaña **Installed**, búscalo, despliega la flecha junto a **Disable** y elige **Uninstall**. Reinicia el IDE.

/// details | Desactivar no es lo mismo que desinstalar
- **Disable** deja el *plugin* instalado pero sin cargarse; puedes reactivarlo cuando quieras.
- **Uninstall** lo elimina por completo.

Desactivar *plugins* que no usas hace que el IDE arranque más rápido y consuma menos memoria.
///

---

### 5. Configurar el JDK

IntelliJ llama **SDK** al kit de desarrollo con el que compila un proyecto. Vamos a registrar el **Temurin 21** de la práctica 02.

#### Paso 1: Añadir el JDK

1. Abre **File → Project Structure** (++ctrl+alt+shift+s++). Si estás en la pantalla de bienvenida, crea antes el proyecto del apartado 6.
2. En **Platform Settings → SDKs**, pulsa **+** y elige **Add JDK from disk…**
3. Selecciona la carpeta del JDK:
    - En **Xubuntu**: `/usr/lib/jvm/temurin-21-jdk-amd64`
    - En **Windows**: `C:\Program Files\Eclipse Adoptium\jdk-21.0.x.y-hotspot`

/// admonition | Es la misma carpeta que `JAVA_HOME`
    type: info

IntelliJ suele **detectar solo** el JDK a partir de `JAVA_HOME`. Si en la lista ya aparece `temurin-21`, no hace falta añadirlo de nuevo.
///

/// admonition | No descargues otro JDK
    type: warning

En el desplegable del SDK, IntelliJ ofrece la opción **Download JDK…**. **No la uses** en esta práctica: trabajamos con el Temurin 21 que instalaste y configuraste tú.
///

---

### 6. Crear, ejecutar y depurar un proyecto Java

#### Paso 1: Crear el proyecto

1. Elige **New Project** en la pantalla de bienvenida (o **File → New → Project**).
2. Rellena el asistente:

| Campo | Valor |
|---|---|
| **Name** | `hola-intellij` |
| **Location** | tu carpeta `ut02p04` (créala si no existe) |
| **Language** | Java |
| **Build system** | IntelliJ (Maven y Gradle los veremos en la UT03) |
| **JDK** | `temurin-21` |
| **Add sample code** | Marcado |

IntelliJ crea esta estructura:

```text
hola-intellij/
├── .idea/              ← configuración del proyecto para IntelliJ
├── src/Main.java       ← código fuente
└── hola-intellij.iml   ← descripción del módulo
```

Al compilar aparecerá también la carpeta `out/`, con los ficheros `.class`.

#### Paso 2: Escribir el programa

Sustituye el contenido de `src/Main.java` por el mismo programa de la práctica 03, cambiando solo el nombre de la clase y el mensaje:

```java
public class Main {
    public static void main(String[] args) {
        String nombre = "tu nombre";   // Cambia por tu nombre
        int suma = 0;

        // Sumamos los números del 1 al 5 para poder seguir el bucle con el depurador
        for (int i = 1; i <= 5; i++) {
            suma = suma + i;
        }

        System.out.println("Hola, " + nombre + ", desde IntelliJ IDEA");
        System.out.println("La suma del 1 al 5 es " + suma);
        System.out.println("Versión de Java: " + System.getProperty("java.version"));
    }
}
```

#### Paso 3: Ejecutar

Pulsa el triángulo verde ▶ junto al método `main` y elige **Run 'Main.main()'** (o ++shift+f10++). La salida aparece en la ventana **Run**, en la parte inferior.

#### Paso 4: Depurar

1. Haz clic en el margen izquierdo de la línea `suma = suma + i;` para poner un **punto de ruptura**.
2. Pulsa el triángulo verde y elige **Debug 'Main.main()'** (o ++shift+f9++).
3. En la ventana **Debug**, observa los valores de `i` y `suma` en el panel **Variables**.
4. Avanza con **Step Over** (++f8++) hasta terminar el bucle.

/// admonition | Cuaderno
    type: tip

Compara la depuración con la de VS Code en la práctica 03: ¿qué ventanas y atajos cambian? ¿Cuál te resulta más cómoda? Anótalo en tu tabla comparativa.
///

---

### 7. Personalizar y automatizar el entorno

#### Paso 1: Tamaño de letra

En **File → Settings → Editor → Font**, cambia **Size** a 16.

#### Paso 2: Acciones al guardar

En **File → Settings → Tools → Actions on Save**, marca:

- **Reformat code**: da formato al código automáticamente.
- **Optimize imports**: elimina los `import` que no se usan.

IntelliJ guarda los cambios automáticamente, así que estas acciones se aplican también cuando cambias de ventana. Pruébalo: desordena la sangría de `Main.java`, pulsa ++ctrl+s++ y observa cómo se corrige.

#### Paso 3: Plantillas de código (*live templates*)

Las plantillas escriben fragmentos de código por ti. En una línea vacía dentro de una clase o un método, escribe la abreviatura y pulsa ++tab++:

| Abreviatura | Genera |
|---|---|
| `psvm` o `main` | El método `public static void main(String[] args)` |
| `sout` | `System.out.println();` |
| `fori` | Un bucle `for` con índice |

Puedes ver y modificar todas las plantillas en **File → Settings → Editor → Live Templates**.

---

### 8. Configurar las actualizaciones

Abre **File → Settings → Appearance & Behavior → System Settings → Updates**:

| Opción | Qué controla |
|---|---|
| **Check IDE updates for** | Si IntelliJ busca nuevas versiones del IDE (y de qué canal: versiones estables o de prueba) |
| **Check for plugin updates** | Si busca actualizaciones de los *plugins* |

Deja ambas activadas, con el canal **Stable Releases**.

/// note | Las actualizaciones no funcionan igual en los dos sistemas
- En **Windows**, IntelliJ descarga e instala sus propias actualizaciones y te avisa con una notificación.
- En **Xubuntu**, lo instalaste con **Snap**, así que se actualiza **solo** en segundo plano. Puedes forzar la actualización con `sudo snap refresh intellij-idea`.
///

---

### 9. El mismo código en dos entornos

Abre en IntelliJ el proyecto **`hola-vscode`** que creaste en la práctica 03 (**File → Open** y selecciona la carpeta). Ejecútalo y comprueba que funciona igual que en VS Code.

Observa qué carpetas y ficheros nuevos ha creado IntelliJ dentro del proyecto (por ejemplo, `.idea/`) y cuáles había creado VS Code (`.vscode/`). Cada entorno guarda su propia configuración, pero el **código fuente** de `src/` es el mismo.

---

### 📑 Entregables

Carpeta **`ut02p04`** comprimida en un fichero llamado **`ut02p04_apellido_nombre.zip`** con:

- El proyecto **`hola-intellij`** completo (sin la carpeta `out/`).
- Un documento **`ut02p04.md`** con las siguientes capturas y una breve explicación de cada una:
    - **Captura 1 (Xubuntu):** salida de `snap list intellij-idea`.
    - **Captura 2 (Windows):** salida de `$env:Path -split ';' | Select-String "JetBrains"`.
    - **Captura 3:** pestaña **Installed** con **Key Promoter X** instalado.
    - **Captura 4:** **Project Structure → SDKs** mostrando Temurin 21.
    - **Captura 5:** ejecución de `Main` en la ventana **Run** (en los dos sistemas).
    - **Captura 6:** depurador detenido en el punto de ruptura, con el panel **Variables** visible.
    - **Captura 7:** **Actions on Save** configurado.
    - **Captura 8:** pantalla **Updates** en cada sistema.
    - **Captura 9:** proyecto `hola-vscode` ejecutándose en IntelliJ.
- Un último apartado en `ut02p04.md`: **«VS Code frente a IntelliJ IDEA»**, con la tabla comparativa del cuaderno pasada a limpio y tu conclusión: ¿cuál usarías para un proyecto Java grande y por qué?
- Foto de la tabla comparativa del **cuaderno**.

/// admonition | Entrega en Moodle
    type: info

Sube el `.zip` a la tarea **«UT02 – Práctica 04: IntelliJ IDEA para Java»** del aula virtual ([aulavirtual.murciaeduca.es](https://aulavirtual.murciaeduca.es)) antes de la fecha indicada en la tarea.
///
---

### ✅ Criterios de corrección

| Criterio | RA – CE | Excelente (100 %) | Adecuado (50 %) | Insuficiente (0 %) | Peso |
|---|---|---|---|---|---|
| Instalación de IntelliJ IDEA en los dos sistemas | RA2 – CE a) | Instalado en ambos desde fuentes oficiales; lanzador en el `PATH` en Windows | Instalado en ambos con alguna evidencia incompleta | Instalado solo en uno o sin evidencias | 15 % |
| Añadir y eliminar *plugins* | RA2 – CE b) | Instala Key Promoter X y desinstala VSCode Keymap con evidencias | Solo instala, o faltan evidencias | No realizado | 10 % |
| JDK configurado como SDK | RA2 – CE a) | Temurin 21 registrado y usado por el proyecto | Usa otro JDK o falta la evidencia | El proyecto no tiene SDK válido | 15 % |
| Proyecto Java creado, ejecutado y depurado | RA2 – CE a) | Proyecto completo; ejecución y depuración con punto de ruptura y variables visibles | Se ejecuta, pero no se depura | No se ejecuta | 15 % |
| Personalización y automatización | RA2 – CE c) | Fuente, acciones al guardar y plantillas usadas y comprobadas | Configuración parcial | Sin personalizar | 10 % |
| Configuración de actualizaciones | RA2 – CE d) | Opciones revisadas en ambos sistemas y diferencias explicadas | Opciones revisadas sin explicación | No realizado | 10 % |
| Mismo código en los dos entornos | RA2 – CE f) | `hola-vscode` ejecutado en IntelliJ y diferencias de configuración explicadas | Ejecutado sin explicación | No realizado | 10 % |
| Comparación VS Code / IntelliJ y cuaderno | RA2 – CE g) | Tabla completa y conclusión razonada | Tabla incompleta o sin conclusión | No presentado | 15 % |
