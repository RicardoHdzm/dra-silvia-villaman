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
img/  logo.png          marca en azul marino sobre blanco, la que usa el sitio
      logo-blanco.png   marca en blanco sobre verde azulado — sin usar, ver abajo
      dra-retrato.png   foto de estudio, la de la ficha
      dra-flyer.webp    el flyer original, material de origen, ya no se usa
```

## Correr en local

```
npx serve .
```

O cualquier servidor estático.

## Concepto

El hero no anuncia, **pregunta**: "¿Qué le ves a la piel?" y seis entradas —mancha,
ronchas, resequedad, lunar, acné, caída de cabello—. Tocas la tuya y bajas al
calendario con el motivo ya elegido. Es tipográfico y sin persona recortada,
a propósito: ese layout de texto-a-la-izquierda y doctora-a-la-derecha es la
plantilla de medio gremio, y era lo que hacía que el hero se sintiera genérico.

El retrato vive ahora en la ficha de "la doctora", que es donde una foto de
retrato de verdad sirve.

Lo que se elige en el hero es un **síntoma**, no un motivo: el motivo depende de
si la consulta es para un niño o para un adulto, así que el mapeo (`SINTOMAS`)
se resuelve en el módulo de agendar, cuando ya se sabe para quién es.

La idea de las dos pieles no se perdió: vive en los dos carriles de
padecimientos, que es donde es una observación útil y no un acertijo.

Debajo del titular hay una línea viva — "Puedes pedir cita desde el viernes 27" —
calculada de la misma constante `HORARIOS`. Dice "pedir", no "reservar",
porque eso es lo que de verdad hace el sitio.

El fondo deriva con el puntero (`--mx`, `--my`), interpolado cuadro por cuadro.
Nada más se mueve.

**La entrada del hero tiene red de seguridad:** los elementos aparecen
escalonados con `animation-fill-mode: backwards`, lo que significa que empiezan
invisibles. A los 1200 ms el JS agrega la clase `listo` al `<html>`, que fuerza
todo a visible. Si algún día la animación no corre, el hero se ve igual en vez
de quedarse en blanco.

## Caminos ya explorados

Para no volver a recorrerlos sin querer:

- **Hero de "las dos pieles"** (titular en dos líneas que se desteñían con el
  cursor, retrato recortado a la derecha, control deslizante). Descartado: era
  la plantilla de medio gremio médico, pedía hover para dar algo y en reposo era
  un póster. Vive en el historial hasta `bb38cb0`.
- **Hero sobre superficie de piel** — rama `hero-piel`, página `hero-piel.html`.
  Ruido celular generado por código en vez del campo lavanda plano. Funciona,
  pesa cero y mide 9.76:1 de contraste, pero se decidió quedarse con el fondo
  liso. La rama queda por si se retoma con una macro real de piel.

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

## La foto de la ficha

`img/dra-retrato.png` (543×846), la de estudio con saco negro.

**No trae transparencia**, pese a ser PNG con canal alfa: el fondo es blanco
opaco. No hace falta recortarla, porque `mix-blend-mode: multiply` funde ese
blanco con la tarjeta y el resultado es limpio.

El encuadre es por CSS, en `.doc-foto`:

```css
aspect-ratio: 434/491;
background-size: 125% auto;
background-position: 40% 9.5%;
```

**El ancho se corta al 88% a propósito.** Del 93.6% en adelante la foto trae el
texto del fondo del set ("S…", "es…"), y si se encuadra más a la derecha,
aparece. Si algún día se reencuadra, respetar ese límite.

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

- **Decidir el color de la marca.** Hay tres paletas entre los materiales: el
  flyer es lavanda y morado, `logo.png` es azul marino sobre blanco, y
  `logo-blanco.png` es blanco sobre **verde azulado `#215F74`**, opaco, sin
  transparencia. El sitio está construido en marino y lavanda. Si el verde
  azulado es el color de la marca, hay que repintar el sitio entero; si solo es
  el fondo con el que exportaron ese archivo, hace falta la marca en vectorial
  o en PNG con transparencia. Sin esa respuesta, `logo-blanco.png` no se puede
  usar en ningún lado.
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
