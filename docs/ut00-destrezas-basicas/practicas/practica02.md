# Práctica UT00P02: Escritura y atajos de teclado

| | |
|---|---|
| **Resultados de aprendizaje y criterios de evaluación** | Actividad de inicio de curso. Prepara las destrezas necesarias para RA2 – CE c) |
| **Duración estimada** | 1 sesión (1 h) |
| **Modalidad** | Individual |
| **Sistemas operativos** | Xubuntu 24.04 o Windows 11 |

---

## 🎯 Objetivos

- Editar texto con agilidad **sin tocar el ratón**.
- Memorizar los atajos de teclado que usarás en cualquier editor de código.

## 🧰 Requisitos

- Haber hecho la **Práctica 01**: existe la carpeta `EDD_2026_2027/UT01_Introduccion/P01`.
- Un editor de texto plano: **Mousepad** (Xubuntu), **Bloc de notas** (Windows), **nano** (terminal) o **VS Code**.

/// note | ¿Por qué importa?
Los programadores y programadoras pasan el día escribiendo. Cada vez que sueltas el teclado para coger el ratón pierdes tiempo y concentración. Estos atajos funcionan igual en casi todos los editores, IDE, navegadores y procesadores de texto.
///

---

## 1. Atajos que vas a practicar

| Acción | Atajo |
|---|---|
| Moverse palabra a palabra | ++ctrl+arrow-left++ / ++ctrl+arrow-right++ |
| Ir al principio / final de la línea | ++home++ / ++end++ |
| Ir al principio / final del documento | ++ctrl+home++ / ++ctrl+end++ |
| Seleccionar hasta el principio / final de la línea | ++shift+home++ / ++shift+end++ |
| Seleccionar palabra a palabra | ++ctrl+shift+arrow-left++ / ++ctrl+shift+arrow-right++ |
| Seleccionar línea a línea | ++shift+arrow-down++ / ++shift+arrow-up++ |
| Seleccionar todo | ++ctrl+a++ |
| Copiar, cortar y pegar | ++ctrl+c++, ++ctrl+x++, ++ctrl+v++ |
| Deshacer y rehacer | ++ctrl+z++, ++ctrl+y++ |
| Buscar | ++ctrl+f++ |
| Guardar | ++ctrl+s++ |

---

## 2. Enunciado

Abre el editor y escribe el siguiente texto, el comienzo de *Don Quijote de la Mancha*:

```text
En un lugar de la Mancha, de cuyo nombre no quiero acordarme, no ha mucho tiempo que vivía un hidalgo de los de lanza en astillero, adarga antigua, rocín flaco y galgo corredor. Una olla de algo más vaca que carnero, salpicón las más noches, duelos y quebrantos los sábados, lentejas los viernes, algún palomino de añadidura los domingos, consumían las tres partes de su hacienda. El resto della concluían sayo de velarte, calzas de velludo para las fiestas, con sus pantuflos de lo mesmo, y los días de entresemana se honraba con su vellorí de lo más fino. Tenía en su casa una ama que pasaba de los cuarenta y una sobrina que no llegaba a los veinte, y un mozo de campo y plaza que así ensillaba el rocín como tomaba la podadera. Frisaba la edad de nuestro hidalgo con los cincuenta años. Era de complexión recia, seco de carnes, enjuto de rostro, gran madrugador y amigo de la caza. Quieren decir que tenía el sobrenombre de «Quijada», o «Quesada», que en esto hay alguna diferencia en los autores que deste caso escriben, aunque por conjeturas verisímiles se deja entender que se llamaba «Quijana». Pero esto importa poco a nuestro cuento: basta que en la narración dél no se salga un punto de la verdad.
```

Guárdalo como **`quijote.txt`** dentro de `EDD_2026_2027/UT01_Introduccion/P01` (++ctrl+s++).

Ahora realiza estas operaciones **sin tocar el ratón**:

1. Pon cada **oración** (lo que termina en punto) en una línea distinta, situándote con ++ctrl+arrow-right++ y pulsando ++enter++.
2. Ve a la primera línea con ++ctrl+home++ y **duplícala**: selecciónala con ++shift+end++, cópiala y pégala debajo.
3. **Mueve** la línea «Frisaba la edad de nuestro hidalgo…» al final del texto: córtala y pégala tras ++ctrl+end++.
4. Busca con ++ctrl+f++ la palabra «Quijana» y sustitúyela por «Quijano».
5. Borra la última línea completa, **deshaz** el borrado con ++ctrl+z++ y **rehazlo** con ++ctrl+y++.
6. Guarda con ++ctrl+s++.

/// admonition | Cuaderno
    type: tip

Copia en tu **cuaderno** la tabla de atajos. Repásala durante las próximas semanas, hasta que los uses sin pensar.
///

---

## 📑 Entregables

- El archivo **`quijote.txt`** final, dentro de `UT01_Introduccion/P01`.

La entrega se hace en la práctica 03, junto con el resto de la unidad.

---

## ✅ Criterios de corrección

| Criterio | Excelente (100 %) | Adecuado (50 %) | Insuficiente (0 %) | Peso |
|---|---|---|---|---|
| Texto completo y guardado en la ruta correcta | Sí | Texto incompleto o en otra carpeta | No entregado | 40 % |
| Operaciones de edición realizadas | Las 6 operaciones se reflejan en el archivo | Faltan operaciones | No realizadas | 60 % |
