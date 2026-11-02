# Práctica UT00P03: Compresión y entrega de prácticas

| | |
|---|---|
| **Resultados de aprendizaje y criterios de evaluación** | Actividad de inicio de curso. Prepara las destrezas necesarias para la entrega de todas las prácticas del módulo |
| **Duración estimada** | 1 sesión (1 h) |
| **Modalidad** | Individual |
| **Sistemas operativos** | Xubuntu 24.04 y Windows 11 |

---

## 🎯 Objetivos

- Comprimir una carpeta en formato `.zip` desde el entorno gráfico y desde la terminal.
- Nombrar correctamente los archivos de entrega.
- Entregar una práctica en el aula virtual.

## 🧰 Requisitos

- Haber hecho las **Prácticas 01 y 02**.
- Conocer tu **NRE** (número regional de estudiante). Aparece en tu matrícula; si no lo conoces, pregúntaselo a tu tutor o tutora.

/// note | ¿Por qué `.zip`?
Las prácticas suelen tener varios archivos y carpetas. Comprimirlos en un único `.zip` permite entregarlos de una vez, ocupan menos espacio y se mantiene la estructura de carpetas. Usamos `.zip` porque se abre en cualquier sistema operativo sin instalar nada.
///

/// admonition | Nombre del archivo de entrega
    type: warning

Salvo que la práctica indique otra cosa, el archivo de entrega se llama **`NRE_UT01.zip`**, sustituyendo `NRE` por tu número. Usa siempre **guiones bajos**, sin espacios, tildes ni eñes: algunos sistemas los convierten en caracteres extraños.
///

---

## 1. Comprobar el contenido

La carpeta `EDD_2026_2027/UT01_Introduccion/P01` debe contener:

- `quijote.txt` (práctica 02).
- `notas_copia.txt` (práctica 01).
- El documento `ut00p01.md` (o PDF) con las capturas de la práctica 01.

Comprueba su contenido desde la terminal:

```bash
ls ~/Escritorio/EDD_2026_2027/UT01_Introduccion/P01
```

---

## 2. Comprimir en Xubuntu

### Opción A: desde el gestor de archivos

1. Abre el gestor de archivos (**Thunar**) y ve a `Escritorio/EDD_2026_2027/UT01_Introduccion`.
2. Haz clic derecho sobre la carpeta **`P01`** y elige **Crear un archivo comprimido…** (o **Comprimir…**).
3. Escribe el nombre **`NRE_UT01`**, elige el formato **`.zip`** y pulsa **Crear**.

### Opción B: desde la terminal

```bash
cd ~/Escritorio/EDD_2026_2027/UT01_Introduccion
zip -r NRE_UT01.zip P01
unzip -l NRE_UT01.zip
```

La opción `-r` (recursiva) incluye todo el contenido de la carpeta. `unzip -l` muestra lo que hay dentro del `.zip` sin descomprimirlo.

/// details | Si el comando `zip` no existe
Instálalo con:

```bash
sudo apt install -y zip unzip
```
///

---

## 3. Comprimir en Windows

### Opción A: desde el Explorador de archivos

1. Abre el Explorador y ve a `Escritorio\EDD_2026_2027\UT01_Introduccion`.
2. Haz clic derecho sobre la carpeta **`P01`** y elige **Comprimir en → Archivo ZIP**.
3. Cambia el nombre del archivo creado a **`NRE_UT01.zip`**.

### Opción B: desde PowerShell

```powershell
Set-Location ([Environment]::GetFolderPath("Desktop"))
Set-Location EDD_2026_2027\UT01_Introduccion
Compress-Archive -Path P01 -DestinationPath NRE_UT01.zip
Get-ChildItem NRE_UT01.zip
```

Para comprobar su contenido, descomprímelo en una carpeta de prueba:

```powershell
Expand-Archive -Path NRE_UT01.zip -DestinationPath prueba_zip
Get-ChildItem -Recurse prueba_zip
```

---

## 4. Entregar en el aula virtual

1. Entra en [aulavirtual.murciaeduca.es](https://aulavirtual.murciaeduca.es) con tu usuario de murciaeduca.
2. Abre el curso **Entornos de Desarrollo** y la tarea **«UT00 – Destrezas básicas»**.
3. Pulsa **Agregar entrega**, arrastra el archivo `NRE_UT01.zip` y pulsa **Guardar cambios**.
4. Comprueba que el **estado de la entrega** indica **Enviado para calificar**.

/// admonition | Revisa siempre tu entrega
    type: danger

Después de entregar, **descarga** tu propio archivo desde la tarea y ábrelo. Un `.zip` vacío, dañado o con los archivos equivocados cuenta como **práctica no entregada**.
///

---

## 📑 Entregables

- El archivo **`NRE_UT01.zip`**, con la carpeta `P01` y todo su contenido, entregado en la tarea **«UT00 – Destrezas básicas»** del aula virtual.

---

## ✅ Criterios de corrección

| Criterio | Excelente (100 %) | Adecuado (50 %) | Insuficiente (0 %) | Peso |
|---|---|---|---|---|
| Nombre del archivo | `NRE_UT01.zip` correcto | Nombre con pequeños errores (espacios, mayúsculas…) | Nombre sin relación con la norma | 20 % |
| Contenido del `.zip` | Contiene la carpeta `P01` con todos los archivos de las prácticas 01 y 02 | Faltan archivos | Vacío o dañado | 50 % |
| Entrega en el aula virtual | Enviada para calificar dentro de plazo | Entregada fuera de plazo | No entregada | 30 % |
