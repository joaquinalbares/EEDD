## UT02 - PRÁCTICA 03 · Instalación y configuración de Visual Studio Code para Java

| | |
|---|---|
| **Resultados de aprendizaje y criterios de evaluación** | RA2 – CE a), RA2 – CE b), RA2 – CE c), RA2 – CE d), RA2 – CE g) |
| **Duración estimada** | 3 sesiones (3 h) |
| **Modalidad** | Individual |
| **Sistemas operativos** | Xubuntu 24.04 y Windows 11 |

---

### 🎯 Objetivos

- Instalar **Visual Studio Code** en Xubuntu y en Windows 11.
- Usar el comando **`code`** desde la terminal y comprobar que está en el `PATH`.
- Añadir y eliminar extensiones, en particular el **Extension Pack for Java**.
- Indicar a VS Code qué JDK debe usar (el **Temurin 21** de la práctica 02).
- Crear, ejecutar y depurar un proyecto Java.
- Personalizar el editor y configurar sus actualizaciones.

### 🧰 Requisitos

- Haber terminado la **Práctica 02**: Temurin 21 instalado y `JAVA_HOME` y `PATH` configurados en los dos sistemas.
- Usuario con permisos de administrador.
- Conexión a Internet.

/// note | ¿Editor o IDE?
Visual Studio Code es, de serie, un **editor de código**: ligero, gratuito y multiplataforma. Se convierte en un **entorno de desarrollo** completo cuando le añades **extensiones** (*plugins*) para un lenguaje: resaltado y autocompletado, compilación, ejecución, depuración, pruebas…

En esta práctica lo convertiremos en un entorno de desarrollo para **Java**.
///

/// admonition | Trabajo en el cuaderno
    type: tip

Durante la práctica, dibuja en tu **cuaderno** un esquema de la ventana de VS Code y señala sus partes: **barra de actividad**, **barra lateral**, **editor**, **panel** (terminal, problemas, consola de depuración) y **barra de estado**. Indica para qué sirve cada una.
///

---

### 1. Instalación en Xubuntu 24.04

Instalaremos VS Code desde el **repositorio oficial de Microsoft**. Así se actualizará junto con el resto del sistema cada vez que hagas `sudo apt upgrade`.

Crea primero la carpeta de la práctica:

```bash
mkdir -p ~/ut02p03 && cd ~/ut02p03
```

#### Paso 1: Actualizar el sistema e instalar las herramientas necesarias

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y wget gpg apt-transport-https
```

#### Paso 2: Añadir la clave de Microsoft

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /usr/share/keyrings/microsoft.gpg
```

#### Paso 3: Añadir el repositorio de VS Code

Crea el fichero `/etc/apt/sources.list.d/vscode.sources` con este contenido:

```bash
sudo tee /etc/apt/sources.list.d/vscode.sources > /dev/null <<'EOF'
Types: deb
URIs: https://packages.microsoft.com/repos/code
Suites: stable
Components: main
Architectures: amd64,arm64,armhf
Signed-By: /usr/share/keyrings/microsoft.gpg
EOF
```

/// details | ¿Por qué este formato y no una línea `deb …` como en cursos anteriores?
Las versiones recientes de Ubuntu usan el formato **deb822** (ficheros `.sources`, con un campo por línea), más fácil de leer que la antigua línea única de los ficheros `.list`. Las dos formas funcionan, pero no debes tener **las dos a la vez** para el mismo repositorio, porque `apt` mostraría avisos de repositorio duplicado.
///

#### Paso 4: Instalar VS Code

```bash
sudo apt update
sudo apt install -y code
```

#### Paso 5: Comprobar la instalación y el comando `code`

```bash
code --version
which code
```

Al instalar con `apt`, el ejecutable queda enlazado en `/usr/bin/code`, una carpeta que ya está en el `PATH`, así que el comando `code` funciona sin hacer nada más.

/// details | Alternativa: instalar el paquete `.deb`
También puedes descargar el `.deb` desde [code.visualstudio.com/download](https://code.visualstudio.com/download) e instalarlo con:

```bash
sudo apt install ./code_*.deb
```

Durante la instalación te preguntará si quieres añadir el repositorio de Microsoft para recibir actualizaciones: responde que **sí**.
///

---

### 2. Instalación en Windows 11

#### Paso 1: Descargar el instalador

Entra en [code.visualstudio.com/download](https://code.visualstudio.com/download) y descarga el **User Installer** de Windows x64. Se instala solo para tu usuario y no necesita permisos de administrador.

#### Paso 2: Ejecutar el instalador

En la pantalla **Seleccione las tareas adicionales**, marca estas opciones:

| Opción | ¿Marcar? | Para qué sirve |
|---|---|---|
| Agregar la acción «Abrir con Code» al menú contextual de **archivo** del Explorador | ✅ Sí | Abrir un fichero en VS Code con el botón derecho |
| Agregar la acción «Abrir con Code» al menú contextual de **directorio** del Explorador | ✅ Sí | Abrir una carpeta completa como proyecto |
| Registrar Code como editor para tipos de archivo admitidos | Opcional | Abrir con doble clic los `.java`, `.md`… |
| **Agregar a PATH** | ✅ **Sí** | Poder usar el comando `code` desde la terminal |

/// details | Alternativa: instalar con `winget`
Desde **PowerShell**:

```powershell
winget install Microsoft.VisualStudioCode
```
///

#### Paso 3: Comprobar la instalación y el comando `code`

Abre una **nueva** ventana de PowerShell (las variables de entorno se leen al abrir la terminal):

```powershell
code --version
where.exe code
```

/// admonition | Si `code` no se reconoce en Windows
    type: warning

Significa que el instalador no añadió VS Code al `PATH`. Tienes dos opciones:

- Volver a ejecutar el instalador y marcar **Agregar a PATH**.
- Añadirlo a mano como en la práctica 02: en **Editar las variables de entorno del sistema → Variables de entorno**, edita el `Path` **de tu usuario** y añade `%LOCALAPPDATA%\Programs\Microsoft VS Code\bin`.

El comando de la paleta **«Shell Command: Install 'code' command in PATH»** que verás en algunos tutoriales **solo existe en macOS**.
///

---

### 3. Primer arranque y uso del comando `code`

#### Paso 1: Abrir VS Code desde la terminal

El comando `code` abre VS Code directamente en una carpeta o fichero. En los dos sistemas, desde la carpeta de la práctica:

| Comando | Qué hace |
|---|---|
| `code .` | Abre la carpeta actual |
| `code fichero.txt` | Abre un fichero concreto |
| `code -n` | Abre una ventana nueva y vacía |
| `code --list-extensions` | Muestra las extensiones instaladas |
| `code --install-extension <id>` | Instala una extensión |
| `code --uninstall-extension <id>` | Desinstala una extensión |

Ejecuta `code .` dentro de `ut02p03`. La primera vez, VS Code te preguntará si **confías en los autores** de la carpeta: responde que sí, porque es tu propia carpeta.

#### Paso 2: Poner VS Code en castellano

1. Pulsa ++ctrl+shift+p++ para abrir la **paleta de comandos**.
2. Escribe `Configure Display Language` y selecciona **Español**.
3. Si no aparece, VS Code te ofrecerá instalar el paquete de idioma **Spanish Language Pack for Visual Studio Code**. Instálalo y reinicia cuando te lo pida.

#### Paso 3: Elegir un tema

Abre la paleta de comandos y busca **Preferencias: Tema de color**. Elige el que prefieras; puedes cambiarlo cuando quieras.

---

### 4. Extensiones: añadir y eliminar módulos

#### Paso 1: Instalar el Extension Pack for Java

1. Pulsa el icono **Extensiones** de la barra de actividad (o ++ctrl+shift+x++).
2. Busca **Extension Pack for Java** (publicado por Microsoft, identificador `vscjava.vscode-java-pack`).
3. Pulsa **Instalar**.

Este paquete instala de una vez varias extensiones:

| Extensión | Para qué sirve |
|---|---|
| Language Support for Java™ by Red Hat | Autocompletado, errores en tiempo real, refactorización |
| Debugger for Java | Depurar con puntos de ruptura |
| Test Runner for Java | Ejecutar pruebas JUnit (lo usaremos en la UT03) |
| Maven for Java | Gestionar proyectos Maven (UT03) |
| Project Manager for Java | Vista **Java Projects** |
| IntelliCode | Sugerencias de código asistidas |

Comprueba desde la terminal qué extensiones tienes instaladas:

```bash
code --list-extensions
```

#### Paso 2: Instalar y desinstalar una extensión desde la terminal

Instala la extensión **Markdown All in One**, que te será útil para escribir la documentación de las prácticas:

```bash
code --install-extension yzhang.markdown-all-in-one
code --list-extensions
```

Ahora **desinstálala** y vuelve a comprobarlo:

```bash
code --uninstall-extension yzhang.markdown-all-in-one
code --list-extensions
```

Vuelve a instalarla (desde la terminal o desde la vista Extensiones); la usarás en el entregable.

---

### 5. Indicar a VS Code qué JDK debe usar

La extensión de Java busca el JDK en `JAVA_HOME` y en el `PATH`. Si hiciste bien la práctica 02, ya encontrará **Temurin 21**. Aun así, vamos a comprobarlo y a fijarlo en la configuración.

#### Paso 1: Comprobar el JDK detectado

Abre la paleta de comandos y ejecuta **Java: Configure Java Runtime**. En la pestaña **Project JDKs** debe aparecer el JDK 21 de Temurin.

#### Paso 2: Fijar el JDK en la configuración de usuario

Abre la paleta de comandos y ejecuta **Preferencias: Abrir configuración de usuario (JSON)**. Añade la configuración de tu sistema dentro de las llaves `{ }` (si ya hay otras líneas, separa cada entrada con una coma).

En **Xubuntu**:

```json
"java.configuration.runtimes": [
    {
        "name": "JavaSE-21",
        "path": "/usr/lib/jvm/temurin-21-jdk-amd64",
        "default": true
    }
]
```

En **Windows** (las barras `\` se escriben dobles en JSON):

```json
"java.configuration.runtimes": [
    {
        "name": "JavaSE-21",
        "path": "C:\\Program Files\\Eclipse Adoptium\\jdk-21.0.x.y-hotspot",
        "default": true
    }
]
```

/// admonition | Usa tu ruta real
    type: warning

Sustituye `jdk-21.0.x.y-hotspot` por el nombre real de la carpeta que tengas en `C:\Program Files\Eclipse Adoptium`. Es el mismo valor que pusiste en `JAVA_HOME` en la práctica 02.
///

---

### 6. Crear, ejecutar y depurar un proyecto Java

#### Paso 1: Crear el proyecto

1. Abre la paleta de comandos y ejecuta **Java: Create Java Project…**
2. Elige **No build tools** (sin Maven ni Gradle; los veremos en la UT03).
3. Selecciona como ubicación la carpeta `ut02p03` y llama al proyecto **`hola-vscode`**.

VS Code crea esta estructura:

```text
hola-vscode/
├── .vscode/settings.json   ← configuración del proyecto
├── bin/                    ← ficheros .class compilados
├── lib/                    ← librerías .jar externas
├── src/App.java            ← código fuente
└── README.md
```

#### Paso 2: Escribir el programa

Sustituye el contenido de `src/App.java` por este:

```java
public class App {
    public static void main(String[] args) {
        String nombre = "tu nombre";   // Cambia por tu nombre
        int suma = 0;

        // Sumamos los números del 1 al 5 para poder seguir el bucle con el depurador
        for (int i = 1; i <= 5; i++) {
            suma = suma + i;
        }

        System.out.println("Hola, " + nombre + ", desde VS Code");
        System.out.println("La suma del 1 al 5 es " + suma);
        System.out.println("Versión de Java: " + System.getProperty("java.version"));
    }
}
```

#### Paso 3: Ejecutar

Pulsa **Run** (encima del método `main`) o el botón ▷ de la esquina superior derecha. La salida aparece en el **terminal** integrado.

#### Paso 4: Depurar

1. Haz clic a la izquierda del número de la línea `suma = suma + i;` para poner un **punto de ruptura** (aparece un círculo rojo).
2. Pulsa **Debug** encima del método `main` (o ++f5++).
3. El programa se detiene en el punto de ruptura. En la vista **Variables** observa los valores de `i` y `suma`.
4. Avanza con **Paso a paso por procedimiento** (++f10++) hasta terminar el bucle.

/// admonition | Cuaderno
    type: tip

Antes de depurar, haz en tu **cuaderno** la **traza** del bucle: una tabla con los valores de `i` y `suma` en cada vuelta. Después, compruébala con el depurador.
///

---

### 7. Personalizar y automatizar el editor

Abre la configuración de usuario en JSON (como en el apartado 5) y añade estas opciones:

```json
"editor.fontSize": 16,
"editor.formatOnSave": true,
"files.autoSave": "afterDelay",
"editor.minimap.enabled": false
```

| Opción | Qué hace |
|---|---|
| `editor.fontSize` | Tamaño de letra del editor |
| `editor.formatOnSave` | Da formato al código automáticamente al guardar |
| `files.autoSave` | Guarda automáticamente los cambios tras una pausa |
| `editor.minimap.enabled` | Oculta el minimapa de la derecha |

Comprueba que funciona: desordena la sangría de `App.java`, guarda con ++ctrl+s++ y observa cómo se corrige sola.

/// details | Configuración de usuario frente a configuración del proyecto
- La **configuración de usuario** se aplica a **todo** lo que abras en VS Code.
- La **configuración del proyecto** (`.vscode/settings.json`) solo se aplica a esa carpeta y **tiene prioridad** sobre la de usuario. Es útil para que todo el equipo de un proyecto trabaje igual, porque se puede subir al repositorio.
///

---

### 8. Configurar las actualizaciones

Abre **Configuración** (++ctrl+comma++) y busca `update`:

| Opción | Valores | Qué controla |
|---|---|---|
| `update.mode` | `default`, `start`, `manual`, `none` | Si VS Code busca e instala actualizaciones de sí mismo |
| `extensions.autoUpdate` | `true`, `onlyEnabledExtensions`, `false` | Si las extensiones se actualizan solas |

Deja `extensions.autoUpdate` activado y revisa el valor de `update.mode` en cada sistema.

/// note | Las actualizaciones no funcionan igual en los dos sistemas
- En **Windows**, VS Code se actualiza **a sí mismo** y te avisa cuando hay una versión nueva.
- En **Xubuntu**, lo instalaste desde un repositorio, así que se actualiza **con el sistema**: `sudo apt update && sudo apt upgrade`. VS Code solo te avisa de que hay una versión nueva.
///

---

### 📑 Entregables

Carpeta **`ut02p03`** comprimida en un fichero llamado **`ut02p03_NRE.zip`** con:

- El proyecto **`hola-vscode`** completo (de uno de los dos sistemas).
- Un documento **`ut02p03.md`**, escrito en VS Code con Markdown All in One, con las siguientes capturas y una breve explicación de cada una:
    - **Captura 1 (Xubuntu):** salida de `code --version` y `which code`.
    - **Captura 2 (Windows):** salida de `code --version` y `where.exe code`.
    - **Captura 3:** salida de `code --list-extensions` antes y después de desinstalar Markdown All in One.
    - **Captura 4:** ventana **Java: Configure Java Runtime** mostrando Temurin 21.
    - **Captura 5:** ejecución de `App.java` en el terminal integrado (en los dos sistemas).
    - **Captura 6:** depurador detenido en el punto de ruptura, con la vista **Variables** visible.
    - **Captura 7:** tu `settings.json` de usuario.
    - **Captura 8:** valor de `update.mode` y `extensions.autoUpdate` en cada sistema.
- Un último apartado en `ut02p03.md`: **«Diferencias entre la instalación en Xubuntu y en Windows»** (instalación, `PATH` y actualizaciones).
- Foto de las páginas del **cuaderno**: esquema de la ventana de VS Code y traza del bucle.

/// admonition | Entrega en Moodle
    type: info

Sube el `.zip` a la tarea **«UT02 – Práctica 03: VS Code para Java»** del aula virtual ([aulavirtual.murciaeduca.es](https://aulavirtual.murciaeduca.es)) antes de la fecha indicada en la tarea.
///

---

### ✅ Criterios de corrección

| Criterio | RA – CE | Excelente (100 %) | Adecuado (50 %) | Insuficiente (0 %) | Peso |
|---|---|---|---|---|---|
| Instalación de VS Code en los dos sistemas, con `code` en el `PATH` | RA2 – CE a) | Instalado en ambos desde las fuentes oficiales; `code` funciona en las dos terminales | Instalado en ambos, pero `code` no funciona en alguno | Instalado solo en uno o sin evidencias | 20 % |
| Añadir y eliminar extensiones | RA2 – CE b) | Extension Pack for Java instalado; instala y desinstala desde la terminal con evidencias | Solo instala, o faltan evidencias | No instala las extensiones | 15 % |
| JDK configurado en VS Code | RA2 – CE a), c) | Temurin 21 detectado y fijado en `java.configuration.runtimes` | Detectado pero no fijado en la configuración | VS Code no encuentra el JDK | 15 % |
| Proyecto Java creado, ejecutado y depurado | RA2 – CE a) | Proyecto completo; ejecución y depuración con punto de ruptura y variables visibles | Se ejecuta, pero no se depura | No se ejecuta | 20 % |
| Personalización y automatización | RA2 – CE c) | Todas las opciones aplicadas y el formato al guardar comprobado | Opciones aplicadas sin comprobación | Sin personalizar | 10 % |
| Configuración de actualizaciones | RA2 – CE d) | Opciones revisadas en ambos sistemas y diferencias explicadas | Opciones revisadas sin explicación | No realizado | 10 % |
| Comparación Xubuntu/Windows y cuaderno | RA2 – CE g) | Diferencias bien explicadas; esquema y traza correctos | Incompletos | No presentado | 10 % |

---
