# utilería.js

**Librería JS funcional de validaciones para formularios — sin frameworks, sin componentes visuales.**

## Portada

`utilería.js` resuelve un problema muy común al construir formularios web: repetir una y otra vez el mismo código de validación (correo, contraseña, edad, campos de solo texto, longitud de números) en cada proyecto. Esta librería junta esas validaciones en **un solo archivo de funciones puras** que reciben un dato y devuelven un resultado (`boolean`, número o `string`), sin depender de ningún framework ni de componentes visuales propios: el HTML y el CSS del proyecto son libres, la librería solo aporta la lógica.

Se integra en tres piezas:

| Archivo | Qué hace |
|---|---|
| `index.html` | Formulario de registro con validación en tiempo real + ventana modal que muestra la edad calculada. |
| `login.html` | Pantalla de acceso que usa `validarCorreo()` y `validarPassword()`. |
| `js/utileria.js` | La librería: 6 funciones obligatorias + 2 funciones propias. |

**Demo en vivo (GitHub Pages):** `https://<tu-usuario>.github.io/<tu-repositorio>/`
**Repositorio:** `https://github.com/<tu-usuario>/<tu-repositorio>`

> Reemplaza los links de arriba por los reales antes de entregar, y déjalos también como comentario en la clase del repositorio y de GitHub Pages, tal como se pide en la consigna.

---

## Instalación

No requiere `npm` ni build. Solo copia la carpeta `js/` a tu proyecto y enlaza el script antes de tu propio código:

```html
<script src="js/utileria.js"></script>
<script>
  // Aquí ya tienes disponible el objeto global Utileria
  console.log(Utileria.validarCorreo("hola@correo.com")); // true
</script>
```

También puede usarse como módulo de Node/CommonJS (útil para pruebas):

```js
const Utileria = require("./js/utileria.js");
Utileria.validarCorreo("hola@correo.com"); // true
```

---

## Uso

Todas las funciones cuelgan del objeto global `Utileria`.

### 1. `validarCorreo(correo)` → `boolean`
Valida el formato `usuario@dominio.extensión`.

```js
Utileria.validarCorreo("ana.perez@gmail.com"); // true
Utileria.validarCorreo("ana.perez@gmail");     // false
Utileria.validarCorreo("ana perez@gmail.com"); // false (espacio)
```

### 2. `soloLetras(texto, permitirEspacios = false)` → `boolean`
Solo letras mayúsculas/minúsculas, acepta vocales acentuadas, ñ y ü. El segundo parámetro (opcional) permite espacios, útil para nombres compuestos.

```js
Utileria.soloLetras("Oaxaca");           // true
Utileria.soloLetras("José Ramón", true); // true
Utileria.soloLetras("Jose3");            // false
```

### 3. `validarLongitud(numero, maxLongitud)` → `boolean`
Valida que la cantidad de dígitos de un número no exceda un máximo. Pensado para teléfonos, códigos postales, tarjetas, etc.

```js
Utileria.validarLongitud(9511234567, 10);   // true  (10 dígitos)
Utileria.validarLongitud("95112345678", 10); // false (11 dígitos)
```

### 4. `calcularEdad(fechaNacimiento)` → `número entero`
Calcula la edad en años cumplidos a partir de una fecha (string `"YYYY-MM-DD"` o `Date`).

```js
Utileria.calcularEdad("2000-07-03"); // 26 (calculado el 2026-07-03)
```

### 5. `esMayorDeEdad(fechaNacimiento)` → `boolean`
Reutiliza `calcularEdad()` para saber si la persona tiene 18 años o más.

```js
Utileria.esMayorDeEdad("2000-07-03"); // true
Utileria.esMayorDeEdad("2015-01-01"); // false
```

### 6. `validarPassword(password)` → `boolean`
Exige mayúscula, minúscula, número, carácter especial y mínimo 8 caracteres.

```js
Utileria.validarPassword("Segura#123"); // true
Utileria.validarPassword("segura123");  // false (sin mayúscula ni especial)
```

### Sección libre — funciones propias

#### `formatearTelefono(numero)` → `string`
Problema que resuelve: los usuarios pegan su teléfono sin formato (`"9511234567"`), lo que dificulta su lectura en pantallas de confirmación o tickets. La función lo convierte a `(951) 123-4567` cuando tiene exactamente 10 dígitos.

```js
Utileria.formatearTelefono("9511234567"); // "(951) 123-4567"
Utileria.formatearTelefono("12345");      // "12345" (no se pudo formatear)
```

#### `capitalizarNombre(texto)` → `string`
Problema que resuelve: los nombres llegan en mayúsculas, minúsculas o mezclados (`"JUAN PEREZ"`, `"juan perez"`). La función normaliza cada palabra con su inicial en mayúscula.

```js
Utileria.capitalizarNombre("JUAN PEREZ");       // "Juan Perez"
Utileria.capitalizarNombre("maría josé lópez"); // "María José López"
```

---

## Integración en el proyecto

- **`index.html`** — formulario con nombre, correo, teléfono, fecha de nacimiento y contraseña. Cada campo se valida en vivo (`soloLetras`, `validarCorreo`, `validarLongitud`, `validarPassword`) y muestra su estado con borde verde/rojo. Al enviar el formulario con todos los datos válidos, se calcula la edad con `calcularEdad()` y se abre una **ventana modal** que muestra la edad y si la persona es mayor de edad (`esMayorDeEdad()`), además de usar las dos funciones propias para formatear el nombre y el teléfono. También incluye una consola en pantalla que imprime cada llamada a la librería, igual que la consola del navegador.
- **`login.html`** — formulario de acceso simulado que valida el correo con `validarCorreo()` y la contraseña con `validarPassword()`, mostrando el resultado en una mini consola en pantalla y con un `alert()` de confirmación.

---

## Capturas de pantalla (consola mostrando resultados)

Salida real de `node` ejecutando la librería (ver `console.log` de cada función):

```
=== PRUEBAS DE UTILERIA.JS ===
validarCorreo("ana.perez@gmail.com") → true
validarCorreo("ana.perez@gmail") → false
soloLetras("Oaxaca") → true
soloLetras("José Ramón", true) → true
validarLongitud(9511234567, 10) → true
calcularEdad("2000-07-03") → 26
esMayorDeEdad("2000-07-03") → true
validarPassword("Segura#123") → true
formatearTelefono("9511234567") → (951) 123-4567
capitalizarNombre("JUAN PEREZ") → Juan Perez
```

> Agrega aquí tus propias capturas de pantalla del navegador: la consola de DevTools (F12) mostrando estos mismos `console.log`, el formulario con los bordes verdes/rojos de validación, el modal con la edad calculada, y el login funcionando. Guarda las imágenes en `/img` y enlázalas así:
>
> ```markdown
> ![Consola del navegador](img/captura-consola.png)
> ![Formulario validado](img/captura-formulario.png)
> ![Modal de edad](img/captura-modal.png)
> ![Login](img/captura-login.png)
> ```

---

## Video corto (demo)

> Graba un video de máx. 1 minuto mostrando: (1) el problema — un formulario sin validar, (2) cómo `utilería.js` lo resuelve en vivo con la consola y los bordes de color, y (3) el resultado — el modal con la edad y el login funcionando. Sube el video (a YouTube, Drive o como archivo en el repo) y coloca el link aquí:
>
> **Video:** `https://<link-a-tu-video>`

---

## Estructura del repositorio

```
/utileria
 ├── README.md
 ├── index.html
 ├── login.html
 ├── /css
 │    └── styles.css
 ├── /js
 │    └── utileria.js
 └── /img
      └── (capturas usadas en este README)
```

## Cómo activar GitHub Pages

1. Sube esta carpeta a un repositorio público en GitHub.
2. Ve a **Settings → Pages**.
3. En **Source**, selecciona la rama `main` y la carpeta `/root`.
4. Guarda y espera un par de minutos: GitHub dará la URL pública (`https://<usuario>.github.io/<repositorio>/`).
5. Verifica que `index.html`, el modal y `login.html` funcionen desde esa URL antes de entregar.

---

## Autor

Proyecto individual — utilería.js, librería de validaciones en JavaScript puro.
