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

`.doc-foto` encuadra el flyer por CSS: `background-size: 238%` y
`background-position: 100% 16.7%` recortan cabeza y hombros, y
`mix-blend-mode: multiply` funde el fondo claro con el de la tarjeta.

Cuando llegue el recorte de verdad —la del saco negro del perfil de Doctoralia,
con transparencia— basta cambiar la `background-image` y volver el
`background-size` a `cover`. El cruce de dos retratos que existía en el hero
anterior ya no aplica: ahora solo hay una foto y vive en la ficha.

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

- **Una foto de retrato de verdad** para la ficha de "la doctora". Hoy es el
  flyer encuadrado por CSS y fundido con `mix-blend-mode: multiply`; de cerca se
  alcanzan a ver fantasmas de las hojitas del flyer. La del saco negro del
  perfil de Doctoralia, recortada con transparencia, es la buena.
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
