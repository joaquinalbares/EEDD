# Práctica UT02P02: Instalación de Java (Eclipse Temurin) y configuración del PATH

| | |
|---|---|
| **Resultados de aprendizaje y criterios de evaluación** | RA1 – CE c), RA1 – CE d), RA2 – CE a), RA2 – CE c) |
| **Duración estimada** | 3 sesiones (3 h) |
| **Modalidad** | Individual |
| **Sistemas operativos** | Xubuntu 24.04 y Windows 11 |

---

## 🎯 Objetivos

- Instalar el kit de desarrollo de Java (**JDK**) de **Eclipse Temurin** en Xubuntu y en Windows 11.
- Entender qué son las variables de entorno **`PATH`** y **`JAVA_HOME`** y para qué sirven.
- Configurar `PATH` y `JAVA_HOME` en los dos sistemas operativos.
- Compilar y ejecutar un programa Java desde la terminal, diferenciando código fuente, *bytecode* y máquina virtual.

## 🧰 Requisitos

- Una máquina (física o virtual) con **Xubuntu 24.04** y otra con **Windows 11**.
- Usuario con permisos de administrador (`sudo` en Xubuntu; cuenta de administrador en Windows).
- Conexión a Internet.

/// note | ¿Por qué Temurin?
Para programar en Java necesitas el **JDK** (*Java Development Kit*), que incluye el compilador `javac`, la máquina virtual `java` y el resto de herramientas de desarrollo.

Puedes usar el JDK oficial de Oracle, pero nosotros usaremos **[Eclipse Temurin](https://adoptium.net/es/temurin/releases/)**, una distribución **libre y gratuita** de OpenJDK mantenida por la fundación Eclipse (proyecto Adoptium), con actualizaciones periódicas.

Instalaremos siempre una versión **LTS** (*Long Term Support*, soporte a largo plazo), porque garantiza estabilidad y actualizaciones de seguridad durante años. En esta práctica usamos **Java 21 LTS**.
///

---

## 1. Conceptos previos: `PATH` y `JAVA_HOME`

Cuando escribes un comando en la terminal (por ejemplo, `javac`), el sistema operativo no busca el programa en todo el disco: lo busca **solo en las carpetas que aparecen en la variable de entorno `PATH`**, en orden, y ejecuta el primero que encuentra. Si la carpeta donde está `javac` no aparece en el `PATH`, verás un error como este:

```text
javac: orden no encontrada                       (Xubuntu)
"javac" no se reconoce como un comando interno… (Windows)
```

La variable **`JAVA_HOME`** guarda la carpeta raíz donde está instalado el JDK. No la usa el sistema operativo, sino otras herramientas de desarrollo (**Maven**, **Gradle**, algunos IDE…) para saber qué Java deben utilizar. Por eso la configuraremos también.

| Variable | Qué contiene | Ejemplo en Xubuntu | Ejemplo en Windows |
|---|---|---|---|
| `JAVA_HOME` | Carpeta raíz del JDK | `/usr/lib/jvm/temurin-21-jdk-amd64` | `C:\Program Files\Eclipse Adoptium\jdk-21.0.x.y-hotspot` |
| `PATH` | Lista de carpetas donde buscar comandos | `…:/usr/lib/jvm/temurin-21-jdk-amd64/bin` | `…;%JAVA_HOME%\bin` |

/// admonition | Trabajo en el cuaderno
    type: tip

Antes de empezar, responde en tu **cuaderno** (a mano):

1. ¿Qué diferencia hay entre el **JDK** y el **JRE**?
2. Dibuja un esquema del proceso `HolaMundo.java` → `javac` → `HolaMundo.class` → `java` (JVM) → salida por pantalla, e indica en cada paso si se trata de **código fuente**, ***bytecode*** (código intermedio) o **ejecución**.
3. Explica con tus palabras qué ocurre cuando escribes `javac` en la terminal y la carpeta del JDK **no** está en el `PATH`.
///

---

## 2. Instalación en Xubuntu 24.04

Crea primero la carpeta de la práctica:

```bash
mkdir -p ~/ut02p02 && cd ~/ut02p02
```

### Paso 1: Actualizar el sistema e instalar las herramientas necesarias

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y wget apt-transport-https gpg
```

### Paso 2: Añadir la clave del repositorio de Adoptium

La clave GPG permite comprobar que los paquetes que descargas proceden realmente de Adoptium y no han sido modificados:

```bash
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/adoptium.gpg > /dev/null
```

### Paso 3: Añadir el repositorio de Temurin

```bash
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
```

/// details | ¿Qué hace `$(awk …)`?
Lee el nombre en clave de tu versión de Ubuntu del fichero `/etc/os-release` (en Xubuntu 24.04 es `noble`) y lo inserta en la línea del repositorio. Así se descarga el paquete compilado para tu versión.

En distribuciones derivadas que **no** son sabores oficiales de Ubuntu (por ejemplo, Linux Mint), hay que cambiar `VERSION_CODENAME` por `UBUNTU_CODENAME`. Xubuntu es un sabor oficial, así que no hace falta.
///

### Paso 4: Instalar el JDK

```bash
sudo apt update
sudo apt install -y temurin-21-jdk
```

### Paso 5: Comprobar la instalación

```bash
java -version
javac -version
```

Ambos comandos deben mostrar la versión **21** y el texto **Temurin**.

/// admonition | ¿Y si tengo varias versiones de Java instaladas?
    type: warning

Si ya tenías otro JDK (por ejemplo, `openjdk` de los repositorios de Ubuntu), puede que `java -version` muestre otra versión. Elige Temurin 21 como versión por defecto con:

```bash
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

Escribe el número de la opción que contiene `temurin-21` y pulsa ++enter++.
///

---

## 3. Configuración de `PATH` y `JAVA_HOME` en Xubuntu

Al instalar con `apt`, el sistema crea enlaces en `/usr/bin`, que ya está en el `PATH`; por eso `java` funciona sin hacer nada más. Aun así, vamos a configurar las variables **a mano**, porque es lo que necesitarás con herramientas como Maven o si algún día instalas un JDK descargado en un `.tar.gz`.

### Paso 1: Localizar la carpeta del JDK

```bash
readlink -f "$(which javac)"
```

La salida será parecida a `/usr/lib/jvm/temurin-21-jdk-amd64/bin/javac`. La carpeta raíz del JDK (tu `JAVA_HOME`) es la parte **anterior** a `/bin/javac`.

### Paso 2: Ver el `PATH` actual

```bash
echo $PATH
```

Las carpetas aparecen separadas por **dos puntos** (`:`).

### Paso 3: Añadir las variables a `~/.bashrc`

El fichero `~/.bashrc` se ejecuta cada vez que abres una terminal. Añade al final las dos variables:

```bash
echo 'export JAVA_HOME=/usr/lib/jvm/temurin-21-jdk-amd64' >> ~/.bashrc
echo 'export PATH="$JAVA_HOME/bin:$PATH"' >> ~/.bashrc
```

/// admonition | Cuidado con `>>`
    type: danger

Usa siempre **dos** símbolos `>>` (añadir al final). Con uno solo (`>`) **borrarías todo el contenido** de tu `~/.bashrc`.
///

### Paso 4: Cargar los cambios y comprobar

```bash
source ~/.bashrc
echo $JAVA_HOME
echo $PATH
which java
java -version
```

`echo $PATH` debe mostrar ahora la carpeta `…/temurin-21-jdk-amd64/bin` **al principio** de la lista.

---

## 4. Instalación en Windows 11

### Paso 1: Descargar el instalador

1. Entra en [https://adoptium.net/es/temurin/releases/](https://adoptium.net/es/temurin/releases/).
2. Selecciona: **Sistema operativo** `Windows`, **Arquitectura** `x64`, **Tipo de paquete** `JDK`, **Versión** `21 - LTS`.
3. Descarga el fichero **`.msi`**.

### Paso 2: Ejecutar el instalador y elegir las opciones

Ejecuta el `.msi` y avanza hasta la pantalla **Configuración personalizada** (*Custom Setup*). Comprueba que están marcadas estas opciones (haz clic en el icono de cada una y elige *Se instalará en la unidad de disco duro local*):

| Opción del instalador | ¿Marcar? | Para qué sirve |
|---|---|---|
| **Add to PATH** | ✅ Sí (viene marcada) | Añade la carpeta `bin` del JDK al `PATH` |
| **Associate .jar** | ✅ Sí (viene marcada) | Abre los `.jar` con Java al hacer doble clic |
| **Set JAVA_HOME variable** | ✅ **Sí (viene desmarcada: márcala)** | Crea la variable `JAVA_HOME` |
| **JavaSoft (Oracle) registry keys** | ❌ No | Solo necesaria para programas antiguos que buscan Java de Oracle |

Termina la instalación.

/// details | Alternativa: instalar desde la terminal con `winget`
Si prefieres la línea de comandos, abre **PowerShell** y ejecuta:

```powershell
winget install EclipseAdoptium.Temurin.21.JDK
```

En este caso **no** se crea `JAVA_HOME`, así que tendrás que configurarla a mano como se explica en el apartado 5.
///

### Paso 3: Comprobar la instalación

Cierra todas las terminales abiertas y abre una **nueva** (las variables de entorno solo se leen al abrir la terminal). En **PowerShell**:

```powershell
java -version
javac -version
```

---

## 5. Configuración de `PATH` y `JAVA_HOME` en Windows 11

Aunque el instalador ya haya creado las variables, debes saber **revisarlas y crearlas a mano**: es una tarea habitual cuando una herramienta no encuentra Java.

### Paso 1: Abrir el editor de variables de entorno

1. Pulsa ++win++ y escribe **«variables de entorno»**.
2. Abre **Editar las variables de entorno del sistema**.
3. Pulsa el botón **Variables de entorno…**

### Paso 2: Revisar o crear `JAVA_HOME`

En **Variables del sistema**:

- Si ya existe `JAVA_HOME`, selecciónala y pulsa **Editar** para comprobar su valor.
- Si no existe, pulsa **Nueva…** y escribe:
    - **Nombre de la variable:** `JAVA_HOME`
    - **Valor de la variable:** la carpeta del JDK, por ejemplo `C:\Program Files\Eclipse Adoptium\jdk-21.0.x.y-hotspot` (usa **Examinar directorio…** para no equivocarte con el número de versión).

/// admonition | Sin `\bin`
    type: warning

`JAVA_HOME` apunta a la carpeta **raíz** del JDK, **sin** `\bin` al final.
///

### Paso 3: Revisar o añadir la entrada en `Path`

1. En **Variables del sistema**, selecciona **Path** y pulsa **Editar**.
2. Comprueba si existe una línea con la carpeta `bin` del JDK. Si no existe, pulsa **Nuevo** y escribe:

    ```text
    %JAVA_HOME%\bin
    ```

3. Usa **Subir** para colocarla **por encima** de cualquier otra entrada de Java (por ejemplo, `C:\Program Files\Common Files\Oracle\Java\javapath`), porque Windows usa la primera que encuentra.
4. Acepta todas las ventanas.

### Paso 4: Comprobar la configuración

Abre una **nueva** ventana de PowerShell:

```powershell
echo $env:JAVA_HOME
$env:Path -split ';'
where.exe java
java -version
javac -version
```

/// details | Si usas el Símbolo del sistema (`cmd`) en lugar de PowerShell
```bat
echo %JAVA_HOME%
echo %PATH%
where java
```
///

---

## 6. Prueba final: compilar y ejecutar en los dos sistemas

Realiza estos pasos **en Xubuntu (carpeta `~/ut02p02`) y en Windows** (crea una carpeta `ut02p02` en tus Documentos).

1. Crea el fichero `HolaMundo.java` con este contenido:

    ```java
    public class HolaMundo {
        public static void main(String[] args) {
            // Mostramos el sistema operativo y la versión de Java que está ejecutando el programa
            System.out.println("¡Hola, mundo desde Java!");
            System.out.println("Sistema operativo: " + System.getProperty("os.name"));
            System.out.println("Versión de Java: " + System.getProperty("java.version"));
            System.out.println("JAVA_HOME: " + System.getenv("JAVA_HOME"));
        }
    }
    ```

2. Compílalo (genera el *bytecode* `HolaMundo.class`):

    ```bash
    javac HolaMundo.java
    ```

3. Ejecútalo con la máquina virtual (sin la extensión `.class`):

    ```bash
    java HolaMundo
    ```

4. Copia el fichero `HolaMundo.class` generado en Xubuntu a Windows (o al revés) y ejecútalo **sin volver a compilar**. Anota en tu cuaderno qué ocurre y **por qué**.

---

## 📑 Entregables

Carpeta **`ut02p02`** comprimida en un fichero llamado **`ut02p02_apellido_nombre.zip`** con:

- `HolaMundo.java`.
- Un documento **`ut02p02.md`** con las siguientes capturas, cada una con una breve explicación:
    - **Captura 1 (Xubuntu):** salida de `java -version` y `javac -version`.
    - **Captura 2 (Xubuntu):** salida de `echo $JAVA_HOME`, `echo $PATH` y `which java`.
    - **Captura 3 (Xubuntu):** últimas líneas de `~/.bashrc` (`tail -n 5 ~/.bashrc`).
    - **Captura 4 (Windows):** ventana de **Variables de entorno** mostrando `JAVA_HOME` y la entrada del `Path`.
    - **Captura 5 (Windows):** salida de `echo $env:JAVA_HOME`, `where.exe java` y `java -version`.
    - **Captura 6 (ambos):** ejecución de `java HolaMundo` en Xubuntu y en Windows.
    - **Captura 7:** ejecución en un sistema del `.class` compilado en el otro.
- Foto de las páginas del **cuaderno** con las respuestas del apartado 1 y la reflexión del apartado 6.

/// admonition | Entrega en Moodle
    type: info

Sube el `.zip` a la tarea **«UT02 – Práctica 02: Java y PATH»** del aula virtual ([aulavirtual.murciaeduca.es](https://aulavirtual.murciaeduca.es)) antes de la fecha indicada en la tarea.
///

---

## ✅ Criterios de corrección

| Criterio | RA – CE | Excelente (100 %) | Adecuado (50 %) | Insuficiente (0 %) | Peso |
|---|---|---|---|---|---|
| Instalación de Temurin 21 en Xubuntu | RA2 – CE a) | Instalado desde el repositorio de Adoptium; `java` y `javac` muestran Temurin 21 | Java instalado pero no es Temurin 21 o falta una comprobación | No instalado o sin evidencias | 20 % |
| Instalación de Temurin 21 en Windows | RA2 – CE a) | Instalado con las opciones correctas; `java` y `javac` muestran Temurin 21 | Instalado pero falta alguna comprobación | No instalado o sin evidencias | 20 % |
| Configuración de `PATH` y `JAVA_HOME` en Xubuntu | RA2 – CE c) | Variables en `~/.bashrc`, cargadas y comprobadas | Variables configuradas con algún error menor | No configuradas | 15 % |
| Configuración de `PATH` y `JAVA_HOME` en Windows | RA2 – CE c) | Variables correctas (`JAVA_HOME` sin `\bin`, `%JAVA_HOME%\bin` en `Path`) y comprobadas | Variables configuradas con algún error menor | No configuradas | 15 % |
| Compilación y ejecución en ambos sistemas | RA1 – CE c) | `HolaMundo` compilado y ejecutado en los dos sistemas | Solo en uno de los sistemas | No se ejecuta | 15 % |
| Cuaderno: conceptos y portabilidad del *bytecode* | RA1 – CE c), RA1 – CE d) | Esquema correcto y explicación clara de por qué el `.class` funciona en ambos sistemas | Respuestas incompletas | No presentado | 15 % |
