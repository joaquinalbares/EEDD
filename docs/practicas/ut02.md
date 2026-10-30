# Práctica UT02: Instalación de Entornos de Ejecución en Xubuntu

---

/// admonition | Actualización Inicial de Repositorios y creación de carpeta de prácticas
    type: warning
Antes de comenzar cualquier instalación, ejecuta siempre en tu terminal el comando de actualización del sistema:

```bash
sudo apt update && sudo apt upgrade -y
```
Y debemos crear la estructura de carpetas siguiente:

```bash 
mkdir -p ~/ut02p01 && cd ~/ut02p01
mkdir -p ~/java && cd ~/java
cd ..
mkdir -p ~/rust && cd ~/rust
cd ..
mkdir -p ~/ruby && cd ~/ruby
cd ..
mkdir -p ~/python && cd ~/python
```
///


/// admonition | Actualización Inicial de Repositorios
    type: hint
    
Los ejemplos de código de esta página se pueden obtener de 

[https://es.wikipedia.org/wiki/Hola_mundo](https://es.wikipedia.org/wiki/Hola_mundo)
///

---

## 1. Java (JDK)

### Paso 1: Instalar OpenJDK
Instala la versión por defecto del Kit de Desarrollo de Java desde los repositorios oficiales de Ubuntu:

```bash title="Terminal - Instalación de Java"
sudo apt install default-jdk -y
```

/// admonition | "Instalación de una versión específica (Opcional)"
    
Si necesitas una versión concreta como **OpenJDK 21**, puedes ejecutar:
```bash
sudo apt install openjdk-21-jdk -y
```
///

### Paso 2: Verificar la instalación
Comprueba que tanto el entorno de ejecución (`java`) como el compilador (`javac`) están correctamente instalados:

```bash title="Comandos de Verificación"
java -version
javac -version
```

### Paso 3: Crear el archivo fuente de java

Crea un archivo llamado `HolaMundo.java`. Puedes usar `nano` o tu editor de texto preferido:

```bash
nano HolaMundo.java

```

Escribe el siguiente código (el nombre de la clase debe coincidir exactamente con el del archivo):

```java
public class HolaMundo {
    public static void main(String[] args) {
        System.out.println("¡Hola, mundo desde Java!");
    }
}

```

Guarda los cambios (**`Ctrl` + `O**`, **`Enter`**) y sal del editor (**`Ctrl` + `X**`).

### Paso 4: Compilar el código

Ejecuta el compilador de Java. Esto generará un archivo `HolaMundo.class` con el *bytecode*:

```bash
javac HolaMundo.java

```

### Paso 5: Ejecutar el programa

Llama a la Máquina Virtual de Java pasando el nombre de la clase (sin extensión `.class`):

```bash
java HolaMundo

```

---

## 2. Python 3 & Pip

Xubuntu incluye Python 3 preinstalado, pero es necesario instalar las herramientas de desarrollo y gestión de paquetes.

### Paso 1: Verificar la versión de Python
Comprueba qué versión de Python 3 está activa en el sistema:

```bash title="Terminal"
python3 --version
```

### Paso 2: Instalar `pip` y entornos virtuales
Instala el gestor de paquetes `pip` y el módulo para entornos virtuales `venv`:

```bash title="Terminal - Instalación de herramientas Python"
sudo apt install python3-pip python3-venv -y
```

### Paso 3: Verificar la instalación de `pip`
Asegúrate de que `pip3` está listo para su uso:

```bash title="Comando de Verificación"
pip3 --version
```

### Paso 4: Crear el archivo del script

Crea un archivo llamado `hola.py`:

```bash
nano hola.py

```

Escribe el código:

```python
print("¡Hola, mundo desde Python!")

```

Guarda y sal del editor.

### Paso 5: Ejecutar el script

Ejecuta el programa directamente con el intérprete:

```bash
python3 hola.py

```

---

## 3. Rust (`rustup`)

La vía oficial y recomendada para instalar Rust en entornos Unix es mediante **`rustup`**.

### Paso 1: Instalar dependencias del sistema
Rust requiere un compilador de C y herramientas de construcción nativas:

```bash title="Terminal - Instalación de dependencias"
sudo apt install build-essential curl -y
```

### Paso 2: Ejecutar el script oficial de instalación
Descarga e inicia el instalador interactivamente:

```bash title="Terminal - Descarga de Rustup"
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

/// admonition | Proceso interactivo

    Cuando la terminal solicite seleccionar el tipo de instalación, presiona ++1++ (opción *default*) y a continuación ++enter++.
///

### Paso 3: Cargar las variables de entorno
Aplica los cambios en el PATH de tu sesión actual sin necesidad de cerrar sesión:

```bash title="Terminal"
source "$HOME/.cargo/env"
```

### Paso 4: Verificar la instalación de Rust y Cargo
Verifica que el compilador `rustc` y el gestor de paquetes y proyectos `cargo` funcionan correctamente:

```bash title="Comandos de Verificación"
rustc --version
cargo --version
```

### Paso 5: Creación y ejecución del archivo "Hola, Mundo".

En Rust tenemos dos opciones para crear nuestro hola mundo particular. haremos las dos.

#### Opción A: Compilación directa con `rustc`

1. **Crear el archivo fuente:**
```bash
nano hola.rs
```


2. **Escribir el código:**
```rust
fn main() {
    println!("¡Hola, mundo desde Rust!");
}
```

3. **Compilar:**
```bash
rustc hola.rs
```

Esto generará un archivo binario ejecutable llamado `hola`.
4. **Ejecutar:**
```bash
./hola
```

#### Opción B: Mediante `cargo` (Gestor de proyectos estándar)

1. **Crear un nuevo proyecto:**
```bash
cargo new hola_cargo
cd hola_cargo

```


2. **Compilar y ejecutar en un solo paso:**
```bash
cargo run

```

---

## 4. Ruby para desarrolladores (usando `rbenv`)

### Paso 1: Instalar dependencias del sistema

Para compilar versiones de Ruby desde el código fuente, necesitas instalar herramientas de compilación y librerías de desarrollo:

```bash
sudo apt update
sudo apt install -y git curl autoconf bison build-essential libssl-dev libreadline-dev zlib1g-dev libyaml-dev libffi-dev libgdbm-dev

```

### Paso 2: Descargar e instalar `rbenv` y `ruby-build`

Ejecuta el script oficial que descarga e instala `rbenv` junto con su complemento de compilación:

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash

```

### Paso 3: Configurar las variables de entorno

Agrega `rbenv` a tu archivo de configuración de terminal (`~/.bashrc`) para que se cargue automáticamente cada vez que abras la consola:

```bash
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc

```

Aplica los cambios en la sesión actual:

```bash
source ~/.bashrc

```

Verifica que `rbenv` se haya instalado correctamente:

```bash
rbenv version
```

### Paso 4: Instalar la versión deseada de Ruby

Puedes ver la lista de versiones disponibles con:

```bash
rbenv install -l
```

Para instalar una versión estable (por ejemplo, Ruby 3.3.0):

```bash
rbenv install 3.3.0
```

*(Este proceso puede tardar un par de minutos mientras se compila).*

### Paso 5: Establecer la versión por defecto

Configura la versión instalada como la versión global del sistema para tu usuario:

```bash
rbenv global 3.3.0

```

### Paso 6: Comprobar la instalación

Verifica la versión activa de Ruby:

```bash
ruby -v

```

### Paso 7: Instalar Bundler

Si vas a trabajar en proyectos con dependencias o gemas, instala **Bundler**:

```bash
gem install bundler

```

---

### Paso 8: Crear un "Hola, Mundo"

Puedes crear un programa rápido de prueba para comprobar que el entorno funciona adecuadamente:

1. Crea un archivo llamado `hola.rb`:
```bash
nano hola.rb
```

2. Escribe el siguiente código dentro del archivo:
```ruby
puts "¡Hola, Mundo!"

```

3. Guarda (`Ctrl + O`, `Enter`) y sal (`Ctrl + X`).

4. Ejecuta el archivo:
```bash
ruby hola.rb

```


---

## 📑 Entregables de la Práctica

/// admonition |  Documentación a Entregar
    type : check
    
CARPETA **'ut02p01'** comprimida con los siguientes contenidos:

- Archivos fuentes en cada carpeta.

- Capturas de pantalla de la terminal mostrando la salida de los siguientes comandos en cada una de las carpetas de los lenguajes:
    - **Captura 1 (Java):** Salida gráfica de los comandos `java -version` y `javac -version`.
    - **Captura 2 (Python):** Salida gráfica de los comandos `python3 --version` y `pip3 --version`.
    - **Captura 3 (Rust):** Salida gráfica de los comandos `rustc --version` y `cargo --version`.
    - **Captura 4 (Ruby):** Salida gráfica de los comandos `ruby -v` y `rbenv version`.



///
