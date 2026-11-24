## UT02 -PRÁCTICA 01 · Instalación de entornos de ejecución en Xubuntu (Python, Rust y Ruby)

| | |
|---|---|
| **Resultados de aprendizaje y criterios de evaluación** | RA2 – CE e), RA1 – CE c) |
| **Duración estimada** | 2 sesiones (2 h) |
| **Modalidad** | Individual |
| **Sistema operativo** | Xubuntu 24.04 |

---

### 🎯 Objetivos

- Instalar desde la terminal las herramientas de tres lenguajes: **Python 3** (con `pip` y `venv`), **Rust** (con `rustup`) y **Ruby** (con `rbenv`).
- Crear, compilar o interpretar y ejecutar un programa «Hola, mundo» en cada lenguaje.
- Diferenciar un lenguaje **compilado** (Rust), que genera un ejecutable, de uno **interpretado** (Python y Ruby).
- Conocer los **gestores de versiones y de proyectos** de cada lenguaje (`rustup`, `cargo`, `rbenv`).

### 🧰 Requisitos

- Una máquina (física o virtual) con **Xubuntu 24.04**.
- Usuario con permisos de administrador (`sudo`).
- Conexión a Internet.

/// note | ¿Por qué tres lenguajes?
Un entorno de desarrollo solo puede compilar o ejecutar un lenguaje si en el sistema están instaladas sus herramientas: compilador, intérprete, gestor de paquetes… En esta práctica las instalas y las usas **desde la terminal**. En las prácticas 03 y 04 verás cómo un IDE se apoya en esas mismas herramientas.

Los ejemplos de «Hola, mundo» en muchos lenguajes están en [es.wikipedia.org/wiki/Hola_mundo](https://es.wikipedia.org/wiki/Hola_mundo).
///

/// admonition | Trabajo en el cuaderno
    type: tip

Durante la práctica, completa en tu **cuaderno** una tabla con una fila por lenguaje y estas columnas: **comando de instalación**, **comando para comprobar la versión**, **comando para ejecutar el programa** y **fichero que se ejecuta** (el código fuente o un ejecutable).

Al terminar, dibuja el esquema `hola.rs` → `rustc` → `hola` → ejecución e indica qué es **código fuente** y qué es **código ejecutable**.
///

---

### 1. Preparación

Actualiza el sistema antes de instalar nada:

```bash
sudo apt update && sudo apt upgrade -y
```

Crea la carpeta de la práctica, con una subcarpeta por lenguaje:

```bash
mkdir -p ~/ut02p01/{python,rust,ruby}
cd ~/ut02p01
```

/// admonition | Trabaja en la carpeta de cada lenguaje
    type: warning

Antes de crear los ficheros de cada apartado, entra en su carpeta (`cd ~/ut02p01/python`, `cd ~/ut02p01/rust` o `cd ~/ut02p01/ruby`). Así la entrega quedará ordenada.
///

---

### 2. Python 3 y `pip`

Xubuntu trae Python 3 instalado, pero no las herramientas para gestionar paquetes y entornos virtuales.

#### Paso 1: Comprobar la versión de Python

```bash
python3 --version
```

#### Paso 2: Instalar `pip` y `venv`

```bash
sudo apt install -y python3-pip python3-venv
pip3 --version
```

/// details | ¿Por qué `pip3 install` da el error `externally-managed-environment`?
Desde Ubuntu 23.04, `pip` no deja instalar paquetes en el Python del sistema, para no romper programas del propio sistema operativo. Los paquetes se instalan dentro de un **entorno virtual**:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install <paquete>
```

En esta práctica no necesitas instalar ningún paquete.
///

#### Paso 3: Crear y ejecutar el programa

```bash
cd ~/ut02p01/python
nano hola.py
```

Escribe el código, guarda (++ctrl+o++, ++enter++) y sal (++ctrl+x++):

```python
print("¡Hola, mundo desde Python!")
```

Ejecútalo con el intérprete:

```bash
python3 hola.py
```

---

### 3. Rust con `rustup`

La forma oficial de instalar Rust es **`rustup`**, que instala el compilador `rustc` y el gestor de proyectos `cargo`, y permite actualizarlos.

#### Paso 1: Instalar las dependencias

Rust necesita un enlazador de C y herramientas de compilación del sistema:

```bash
sudo apt install -y build-essential curl
```

#### Paso 2: Ejecutar el instalador oficial

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

/// note | Instalación interactiva
Cuando el instalador te pregunte el tipo de instalación, pulsa ++1++ (*default*) y después ++enter++.
///

#### Paso 3: Cargar las variables de entorno

El instalador añade `~/.cargo/bin` al `PATH`, pero solo en las terminales nuevas. Para usarlo en la actual:

```bash
source "$HOME/.cargo/env"
```

#### Paso 4: Comprobar la instalación

```bash
rustc --version
cargo --version
```

#### Paso 5: Compilar y ejecutar «Hola, mundo»

Lo harás de dos formas.

##### Opción A: compilar directamente con `rustc`

1. Crea el fichero fuente:

    ```bash
    cd ~/ut02p01/rust
    nano hola.rs
    ```

2. Escribe el código:

    ```rust
    fn main() {
        println!("¡Hola, mundo desde Rust!");
    }
    ```

3. Compílalo. Se genera un fichero ejecutable llamado `hola`:

    ```bash
    rustc hola.rs
    ls -l
    ```

4. Ejecútalo:

    ```bash
    ./hola
    ```

##### Opción B: crear un proyecto con `cargo`

1. Crea el proyecto y entra en su carpeta:

    ```bash
    cd ~/ut02p01/rust
    cargo new hola_cargo
    cd hola_cargo
    ```

2. Abre `src/main.rs` y cambia el mensaje por `¡Hola, mundo desde Cargo!`.

3. Compila y ejecuta en un solo paso:

    ```bash
    cargo run
    ```

    El ejecutable queda en `target/debug/hola_cargo`.

---

### 4. Ruby con `rbenv`

**`rbenv`** permite instalar varias versiones de Ruby y elegir cuál usar, sin tocar el Ruby del sistema. Su complemento **`ruby-build`** descarga y compila Ruby desde el código fuente.

#### Paso 1: Instalar las dependencias

```bash
sudo apt install -y git curl autoconf bison build-essential libssl-dev libreadline-dev zlib1g-dev libyaml-dev libffi-dev libgdbm-dev
```

#### Paso 2: Instalar `rbenv` y `ruby-build`

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
```

#### Paso 3: Configurar las variables de entorno

Añade `rbenv` al final de tu `~/.bashrc` para que se cargue en cada terminal:

```bash
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
rbenv --version
```

/// admonition | Cuidado con `>>`
    type: danger

Usa siempre **dos** símbolos `>>` (añadir al final). Con uno solo (`>`) **borrarías todo el contenido** de tu `~/.bashrc`.
///

#### Paso 4: Instalar Ruby

Consulta las versiones estables disponibles:

```bash
rbenv install -l
```

Instala la **3.3.0** (o la versión 3.3 más reciente de la lista):

```bash
rbenv install 3.3.0
```

/// note | Paciencia
`rbenv` compila Ruby desde el código fuente: puede tardar entre 3 y 10 minutos. Si falla con errores sobre `psych` u `openssl`, repite el paso 1: falta alguna librería.
///

#### Paso 5: Elegir la versión por defecto y comprobarla

```bash
rbenv global 3.3.0
rbenv version
ruby -v
```

#### Paso 6: Instalar Bundler

**Bundler** gestiona las dependencias (*gemas*) de los proyectos Ruby:

```bash
gem install bundler
```

#### Paso 7: Crear y ejecutar el programa

```bash
cd ~/ut02p01/ruby
nano hola.rb
```

```ruby
puts "¡Hola, mundo desde Ruby!"
```

```bash
ruby hola.rb
```

---

### 📑 Entregables

Carpeta **`ut02p01`** comprimida en un fichero llamado **`ut02p01_apellido_nombre.zip`** con:

- `python/hola.py`, `rust/hola.rs`, el proyecto `rust/hola_cargo` (sin la carpeta `target/`) y `ruby/hola.rb`.
- Un documento **`ut02p01.md`** con las siguientes capturas, cada una con una breve explicación:
    - **Captura 1 (Python):** salida de `python3 --version`, `pip3 --version` y `python3 hola.py`.
    - **Captura 2 (Rust):** salida de `rustc --version`, `cargo --version`, `ls -l` tras compilar con `rustc` y `./hola`.
    - **Captura 3 (Rust):** salida de `cargo run`.
    - **Captura 4 (Ruby):** salida de `rbenv version`, `ruby -v` y `ruby hola.rb`.
- Foto de las páginas del **cuaderno** con la tabla de comandos y el esquema de Rust.

/// admonition | Entrega en Moodle
    type: info

Sube el `.zip` a la tarea **«UT02 – Práctica 01: Entornos de ejecución»** del aula virtual ([aulavirtual.murciaeduca.es](https://aulavirtual.murciaeduca.es)) antes de la fecha indicada en la tarea.
///

---

### ✅ Criterios de corrección

| Criterio | RA – CE | Excelente (100 %) | Adecuado (50 %) | Insuficiente (0 %) | Peso |
|---|---|---|---|---|---|
| Python: herramientas instaladas y programa ejecutado | RA2 – CE e) | `pip` y `venv` instalados; versiones y ejecución con evidencias | Falta alguna evidencia | No realizado | 20 % |
| Rust: instalación con `rustup` y compilación con `rustc` y con `cargo` | RA2 – CE e), RA1 – CE c) | Las dos formas funcionan, con evidencias del ejecutable generado | Solo una de las dos | No realizado | 30 % |
| Ruby: `rbenv`, Ruby 3.3 y programa ejecutado | RA2 – CE e) | `rbenv version` y `ruby -v` correctos; programa ejecutado | Ruby instalado sin `rbenv` o falta alguna evidencia | No realizado | 25 % |
| Organización de la entrega | — | Carpetas por lenguaje, nombre del `.zip` correcto, sin `target/` | Ficheros desordenados pero completos | Faltan ficheros fuente | 10 % |
| Cuaderno: tabla de comandos y esquema de Rust | RA1 – CE c) | Tabla completa y esquema que distingue fuente y ejecutable | Incompletos | No presentado | 15 % |

---