# Práctica UT00P01: Uso de la terminal en Xubuntu y en Windows (PowerShell)

| | |
|---|---|
| **Resultados de aprendizaje y criterios de evaluación** | Actividad de inicio de curso. Prepara las destrezas necesarias para RA2 – CE a) y RA2 – CE c) |
| **Duración estimada** | 2 sesiones (2 h): 1 h para Xubuntu y 1 h para PowerShell |
| **Modalidad** | Individual |
| **Sistemas operativos** | Xubuntu 24.04 y Windows 11 |

---

## 🎯 Objetivos

- Abrir la terminal en Xubuntu y en Windows.
- Moverte por las carpetas y crear, copiar, renombrar y borrar archivos y carpetas **sin usar el ratón**.
- Conocer los comandos equivalentes en **Bash** (Linux) y **PowerShell** (Windows).
- Crear la estructura de carpetas que usarás durante todo el curso.

## 🧰 Requisitos

- Una máquina con Xubuntu 24.04 y otra con Windows 11.

/// note | ¿Por qué la terminal?
Como programador o programadora usarás la terminal a diario: para instalar herramientas, compilar, ejecutar pruebas, trabajar con Git o conectarte a servidores. Muchas de estas tareas **solo** se pueden hacer desde la terminal, y todas son más rápidas desde ella cuando la dominas.
///

---

## Parte A: la terminal de Xubuntu (Bash)

Abre la terminal con ++ctrl+alt+t++ (o desde el menú de aplicaciones: **Sistema → Terminal**).

#### Ejercicio 1: ¿Dónde estoy?

Averigua en qué directorio (carpeta) estás en este momento:

```bash
pwd
```

Al abrir la terminal siempre empiezas en tu **carpeta personal** (`/home/tu_usuario`), que también se representa con el símbolo `~`.

#### Ejercicio 2: Listar el contenido

Lista los archivos y carpetas del directorio actual:

```bash
ls
```

#### Ejercicio 3: Archivos ocultos

Lista también los archivos **ocultos**, que en Linux son los que empiezan por un punto:

```bash
ls -a
```

Prueba también `ls -la`: ¿qué información nueva aparece?

#### Ejercicio 4: Crear la carpeta del curso en el escritorio

Entra en el escritorio y crea la carpeta del módulo:

```bash
cd ~/Escritorio
mkdir EDD_2026_2027
```

/// admonition | ¿Escritorio o Desktop?
    type: warning

Si tu Xubuntu está instalado en inglés, la carpeta se llama `~/Desktop`. Compruébalo con `ls ~`.
///

#### Ejercicio 5: Entrar en la carpeta

```bash
cd EDD_2026_2027
pwd
```

#### Ejercicio 6: Crear una carpeta dentro

```bash
mkdir UT01_Introduccion
ls
```

#### Ejercicio 7: Subir y bajar de nivel

Vuelve al directorio anterior (el escritorio) y comprueba dónde estás:

```bash
cd ..
pwd
```

Vuelve a entrar en la carpeta del curso:

```bash
cd EDD_2026_2027
```

#### Ejercicio 8: Estructura de carpetas del curso

Dentro de `EDD_2026_2027`, crea una carpeta por unidad y, dentro de cada una, una carpeta **`P01`**. La opción `-p` crea a la vez las carpetas intermedias que no existan:

```bash
mkdir -p UT01_Introduccion/P01
mkdir -p UT02_IDE/P01
mkdir -p UT03_Pruebas/P01
mkdir -p UT04_Git_Refactor/P01
mkdir -p UT05_UML_Clases/P01
mkdir -p UT06_UML_Comportamiento/P01
```

Comprueba el resultado:

```bash
ls -R
```

/// details | Para ir más rápido: autocompletar con el tabulador
Escribe `cd UT0` y pulsa ++tab++ dos veces: la terminal te muestra todas las opciones que empiezan así. Escribe una letra más y vuelve a pulsar ++tab++ para que complete el nombre. Usar el tabulador **evita errores** al escribir nombres largos.

Con ++arrow-up++ recuperas los comandos que escribiste antes.
///

#### Ejercicio 9: Crear un archivo

Entra en `UT01_Introduccion/P01` y crea un archivo vacío:

```bash
cd UT01_Introduccion/P01
touch notas.txt
ls
```

#### Ejercicio 10: Copiar un archivo

```bash
cp notas.txt notas_copia.txt
ls
```

#### Ejercicio 11: Renombrar (mover) un archivo

En Linux, renombrar es **mover** un archivo a otro nombre:

```bash
mv notas.txt final.txt
ls
```

#### Ejercicio 12: Borrar un archivo

```bash
rm final.txt
ls
```

/// admonition | `rm` no tiene papelera
    type: danger

Lo que borras con `rm` **no va a la papelera**: desaparece. Revisa siempre el comando antes de pulsar ++enter++.
///

---

## Parte B: PowerShell en Windows 11

En Windows usaremos **PowerShell** dentro de la aplicación **Terminal**. Ábrela con ++win+x++ y elige **Terminal**, o pulsa ++win++, escribe `terminal` y pulsa ++enter++. Comprueba que la pestaña indica **PowerShell**.

/// note | Comandos de PowerShell y alias
Los comandos de PowerShell (*cmdlets*) tienen la forma **Verbo-Sustantivo**: `Get-Location`, `New-Item`, `Copy-Item`… Para facilitar el cambio desde Linux, PowerShell incluye **alias** con los nombres de Bash: `pwd`, `ls`, `cd`, `cp`, `mv`, `rm` y `mkdir` también funcionan.

En esta parte usarás el **nombre completo** de cada *cmdlet*, para que aprendas cómo se llaman en realidad.
///

### Tabla de equivalencias

| Acción | Bash (Linux) | PowerShell (Windows) | Alias en PowerShell |
|---|---|---|---|
| Ver dónde estoy | `pwd` | `Get-Location` | `pwd` |
| Listar | `ls` | `Get-ChildItem` | `ls`, `dir` |
| Listar ocultos | `ls -a` | `Get-ChildItem -Force` | `ls -Force` |
| Cambiar de carpeta | `cd carpeta` | `Set-Location carpeta` | `cd` |
| Crear carpeta | `mkdir -p a/b` | `New-Item -ItemType Directory -Path a\b` | `mkdir a\b` |
| Crear archivo vacío | `touch f.txt` | `New-Item -ItemType File -Name f.txt` | `ni f.txt` |
| Copiar | `cp a b` | `Copy-Item a b` | `cp`, `copy` |
| Mover o renombrar | `mv a b` | `Move-Item a b` o `Rename-Item a b` | `mv`, `move`, `ren` |
| Borrar | `rm f` | `Remove-Item f` | `rm`, `del` |
| Ver el árbol de carpetas | `ls -R` | `Get-ChildItem -Recurse` | `tree` (comando de Windows) |
| Ayuda de un comando | `man ls` | `Get-Help Get-ChildItem` | `help` |

#### Ejercicio 13: ¿Dónde estoy?

```powershell
Get-Location
```

En Windows empiezas en tu carpeta de usuario (`C:\Users\tu_usuario`). Fíjate en que las rutas usan la barra invertida `\`.

#### Ejercicio 14: Listar, también los ocultos

```powershell
Get-ChildItem
Get-ChildItem -Force
```

¿Qué columna nueva aparece en la segunda orden y qué significan sus letras (`d`, `a`, `h`…)?

#### Ejercicio 15: Ir al escritorio

```powershell
Set-Location ([Environment]::GetFolderPath("Desktop"))
Get-Location
```

/// details | ¿Por qué no basta con `cd Desktop`?
En muchos equipos con Windows 11, el escritorio está **sincronizado con OneDrive** y su ruta real es `C:\Users\tu_usuario\OneDrive\Escritorio` (o `\Desktop`). La expresión `[Environment]::GetFolderPath("Desktop")` pregunta a Windows dónde está el escritorio, sea cual sea su ruta.
///

#### Ejercicio 16: Crear la carpeta del curso y la estructura de unidades

```powershell
New-Item -ItemType Directory -Name EDD_2026_2027
Set-Location EDD_2026_2027
New-Item -ItemType Directory -Path UT01_Introduccion\P01
New-Item -ItemType Directory -Path UT02_IDE\P01
New-Item -ItemType Directory -Path UT03_Pruebas\P01
New-Item -ItemType Directory -Path UT04_Git_Refactor\P01
New-Item -ItemType Directory -Path UT05_UML_Clases\P01
New-Item -ItemType Directory -Path UT06_UML_Comportamiento\P01
tree
```

#### Ejercicio 17: Subir de nivel y volver

```powershell
Set-Location ..
Get-Location
Set-Location EDD_2026_2027\UT01_Introduccion\P01
```

El tabulador también autocompleta en PowerShell: escribe `Set-Location EDD` y pulsa ++tab++.

#### Ejercicio 18: Crear, copiar, renombrar y borrar archivos

```powershell
New-Item -ItemType File -Name notas.txt
Copy-Item notas.txt notas_copia.txt
Rename-Item notas.txt final.txt
Get-ChildItem
Remove-Item final.txt
Get-ChildItem
```

#### Ejercicio 19: Descubrir comandos

PowerShell te ayuda a encontrar los comandos que no conoces:

```powershell
Get-Command -Noun Item
Get-Help Copy-Item -Examples
Get-Alias ls
```

¿Qué muestra cada uno?

---

/// admonition | Trabajo en el cuaderno
    type: tip

Responde en tu **cuaderno**:

1. Copia la **tabla de equivalencias** Bash–PowerShell con los comandos que has usado.
2. En Xubuntu, crea `Prueba.txt` y `prueba.txt` en la misma carpeta. Haz lo mismo en Windows. ¿Qué ocurre en cada sistema? ¿Por qué? (Pista: **mayúsculas y minúsculas**.)
3. ¿Qué diferencias hay entre las rutas de Linux y de Windows? Piensa en la barra separadora, la raíz (`/` frente a `C:\`) y la carpeta personal.
///

---

## 📑 Entregables

Un documento **`ut00p01.md`** (o PDF) con:

- **Captura 1 (Xubuntu):** salida de `ls -R` dentro de `EDD_2026_2027`.
- **Captura 2 (Xubuntu):** ejercicios 9 a 12, con los `ls` que muestran cada cambio.
- **Captura 3 (Windows):** salida de `tree` dentro de `EDD_2026_2027`.
- **Captura 4 (Windows):** ejercicio 18 completo.
- **Captura 5 (Windows):** salida de los tres comandos del ejercicio 19.
- Foto de las páginas del **cuaderno**.

La entrega se hace en la práctica 03, junto con el resto de la unidad.

---

## ✅ Criterios de corrección

| Criterio | Excelente (100 %) | Adecuado (50 %) | Insuficiente (0 %) | Peso |
|---|---|---|---|---|
| Comandos de Bash (ejercicios 1-12) | Todos realizados y la estructura de carpetas es correcta | Faltan ejercicios o hay errores en la estructura | No realizado | 35 % |
| Comandos de PowerShell (ejercicios 13-19) | Todos realizados con los *cmdlets* completos | Faltan ejercicios o se usan solo los alias | No realizado | 35 % |
| Cuaderno | Tabla completa y respuestas razonadas | Respuestas incompletas | No presentado | 30 % |
