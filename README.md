# Dra. Silvia Villaman — sitio

Dermatóloga pediátrica, clínica y estética. Culiacán, Sin.

Estático: HTML + CSS + JS vanilla. Sin build, sin dependencias.
Las únicas peticiones externas son las fuentes de Google (Plus Jakarta Sans e Instrument Serif).

## Archivos

```
index.html                 home completa
aviso-de-privacidad.html   borrador, pendiente de revisión legal
favicon.svg
robots.txt
img/  logo.png · dra-flyer.webp
```

## Correr en local

```
npx serve .
```

O cualquier servidor estático.

## Concepto

"Las dos pieles de tu casa". El sitio no vende una especialidad sino dos consultas
con la misma doctora: la piel de los niños y la de sus papás.

El hero gira alrededor de una sola variable CSS `--t` (0 = niños, 1 = adultos).
De ella dependen el color de cada línea del titular, cuál párrafo se ve, qué CTA
se enciende, la atmósfera del fondo y el desplazamiento de la foto. Se controla
con el cursor en escritorio, al pasar o enfocar cada botón, y con el control
arrastrable, que es el único camino en táctil y con teclado.

Los dos CTA del hero no abren WhatsApp: bajan a la sección de agendar **con el
lado ya elegido**, así que el hero es el primer paso del flujo y no un póster.
El camino directo a WhatsApp sigue existiendo en la sección de agendar, en
"dónde atiende" y en el cierre.

Debajo del titular hay una línea viva — "Puedes pedir cita desde el viernes 27" —
calculada de la misma constante `HORARIOS`. Se actualiza sola y dice "pedir",
no "reservar", porque eso es lo que de verdad hace el sitio.

El lado activo del hero es un estado discreto (`data-lado` en la sección:
`ninos` · `neutro` · `adultos`) y el CSS decide por clase qué texto, qué color
de titular y qué CTA se enciende. **No usar aritmética de opacidades para
esto**: la versión anterior lo hacía y, con el puntero a media travesía, dos
párrafos quedaban encimados.

Secciones: hero · la doctora · padecimientos (dos carriles) · la consulta paso a
paso · **agendar** · opiniones · preguntas · dónde y cuánto · cierre · pie.

## Cómo funciona "agendar"

No hay backend ni reserva real. El calendario sirve para **armar el mensaje de
WhatsApp**: eliges para quién es, día, hora y motivo, ves el mensaje completo
antes de mandarlo, y el botón abre WhatsApp con todo ya escrito. La cita queda
en firme cuando ella contesta, y eso se dice en la propia sección.

No se envía nada a ningún servidor y no se guarda nada en el navegador.

El horario vive en una sola constante al inicio de ese bloque de JS:

```js
var MANANA = ['10:00','10:30','11:00','11:30','12:00','12:30','13:00'];
var TARDE  = ['16:00','16:30','17:00','17:30','18:00','18:30'];
var HORARIOS = {
  0: [],                     // domingo: cerrado
  1: MANANA.concat(TARDE),   // lunes a viernes: dos bloques
  ...
  6: MANANA                  // sabado: solo manana
};
var DIAS_MINIMOS = 1;   // para hoy ya no se agenda
var DIAS_MAXIMOS = 60;
```

Un día sin horas sale deshabilitado en el calendario. Los motivos también son
dos listas ahí mismo, una por cada lado del concepto.

El calendario se maneja con teclado: flechas para moverse entre días, Enter para
elegir. Los días deshabilitados se saltan.

## Cómo activar la segunda foto del hero

El hero puede cruzar dos retratos: bata en el lado "niños", saco negro en el de
"adultos". Falta el archivo. Cuando exista:

1. Guarda el recorte con fondo transparente en `img/dra-blazer.png`.
2. En `index.html`, agrega el atributo al contenedor del retrato:
   `<div class="portrait" data-alt-photo="img/dra-blazer.png">`
3. Ajusta el encuadre en `.photo.alt { background-position }`.

Sin el atributo no se pide el archivo, así que no hay petición fallida.

## Datos tomados del perfil de Doctoralia

`doctoralia.com.mx/perfil/silvia-villaman` — verificar con ella antes de publicar.

- Médico cirujano por la Universidad Autónoma de Sinaloa.
- Dermatología Pediátrica por el **Hospital Infantil de México Federico Gómez**.
  Es la credencial más fuerte que tiene y el flyer ni la menciona.
- Consultorio: Pedro María Anaya 2188, Culiacán.
- Primera vez $1,400 · subsecuente $1,200 · consulta en línea $1,400.
- 4 de 5 estrellas en 5 opiniones.
- Ofrece consulta en línea.

## Pendientes antes de producción

Que dependen de la doctora:

- **Cédula: hay discrepancia.** El flyer dice `138224021`; Doctoralia dice
  `13824021` (un dígito menos) más `9532246` y `12820870`. En el sitio quedaron
  las tres de Doctoralia. Confirmar cuál es cuál.
- **Horario de consulta — urgente.** El calendario de "agendar" muestra horas
  **inventadas**: dos bloques de lunes a viernes (10:00–13:00 y 16:00–18:30) y
  sábado solo por la mañana. Es el patrón más común en consultorio privado, pero
  sigue siendo un supuesto. Son creíbles y por eso mismo peligrosas: si alguien
  pide las 18:00 y ella no atiende a esa hora, la primera impresión ya se perdió.
  Cambiar `MANANA`, `TARDE` y `HORARIOS` antes de enseñar el sitio a nadie.
  La tarjeta de costos también dice "Por confirmar" en el renglón de horario.
- **Correo de contacto** para el aviso de privacidad y los derechos ARCO.
- **Revisión legal del aviso.** La consulta médica maneja datos personales
  sensibles; el borrador sigue la estructura de la LFPDPPP pero necesita abogado.
- **Opiniones.** La sección resume los temas que se repiten, no cita textual.
  Para citas literales hace falta su permiso y acreditar a Doctoralia.
  En las opiniones hay una queja de una cita confirmada que no se respetó; el
  paso 02 de "la consulta" está escrito para atender eso.

De producción gráfica:

- **`img/dra-blazer.png`** — recorte con transparencia de la foto del saco negro
  (la misma del perfil de Doctoralia).
- **Foto de bata recortada.** Hoy es el flyer encuadrado por CSS y fundido con
  `mix-blend-mode: multiply`. Se ven fantasmas de las hojitas del flyer de cerca.
- **Logo con transparencia.** `logo.png` trae fondo blanco, resuelto también con
  `multiply`. Falla si el fondo deja de ser claro.
- **Imagen de Open Graph** — la meta existe, falta el archivo.

Técnicos:

- **Dominio** para la línea `Sitemap:` de `robots.txt` y para `sitemap.xml`.
- Cuando el sitio pase de tres páginas, mover los estilos a `css/style.css`:
  hoy cada página trae los suyos y el aviso duplica los tokens de color.

## Paleta

| Uso | Valor |
|---|---|
| Texto y estructura | `#1B2F57` |
| Texto secundario | `#44507A` |
| Fondo | `#F5F1FB` |
| Lavanda | `#7E6FC4` / `#5B4FA3` |
| Acento (solo CTA) | `#E2A13C` / `#C9871F` |
| Pie | `#16294D` |
