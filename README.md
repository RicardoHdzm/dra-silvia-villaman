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
      dra-retrato.webp  foto de estudio, la de la ficha (42 KB, la que carga)
      dra-retrato.png   el original sin comprimir, material de origen
      dra-flyer.webp    el flyer original, material de origen, ya no se usa
```

## En línea

**https://ricardohdzm.github.io/dra-silvia-villaman/**

Es una **previa**, no el sitio final. Lleva `noindex, nofollow` a propósito:
el horario del calendario sigue inventado y la cédula sin confirmar, así que
si Google lo indexa alguien puede llegar al consultorio a una hora que no
existe. **Quitar esa etiqueta solo cuando esos datos estén verificados y la
doctora apruebe el contenido.**

El repositorio tuvo que volverse **público** para poder usar GitHub Pages:
en cuenta personal, Pages sobre repos privados requiere plan de pago. Eso
significa que el código y todo el historial son públicos. Si más adelante se
prefiere ocultarlos, Cloudflare Pages y Netlify despliegan desde repos
privados en su plan gratuito.

## Correr en local

```
npx serve .
```

O cualquier servidor estático.

## El hero: una pantalla

Una sola pantalla de alto, con el titular centrado, el botón que lleva al menú
de síntomas y la credencial debajo. Nada se mueve con el scroll.

**Por qué es así.** Antes era un recorrido: el scroll pasaba por varias paradas
y cada una repintaba el fondo. Se quitó porque se trababa en Chrome y en
teléfono, y no era culpa de las paradas sino del fondo que movían. Eran seis
elementos grandes con `filter:blur()` y `mix-blend-mode`, recompuestos en cada
cuadro durante casi cuatro pantallas de scroll. Un `blur` sobre una capa
grande no se rasteriza una vez y ya: cada vez que algo de esa capa cambia —su
posición, su color, su `border-radius`— el navegador la vuelve a rasterizar
entera, y eso no lo hace la GPU gratis.

La regla que queda de aquello: **lo que se mueve no lleva blur, y lo que lleva
blur no se mueve.** Si algún día vuelve a animarse el fondo, hay que medirlo
antes, no suponerlo.

**El fondo de ahora son dos capas de `.campo`**, no una. Degradados radiales
morados, sin blur y sin mezcla: un radial ya nace difuso porque su última
parada es transparente, así que da la misma nube sin costar lo mismo.

Dos y no una por una razón concreta: **una sola capa que se traslada se ve
como una foto que se desliza**. El fondo se mueve pero no cambia, y eso se
lee como quietud. Con dos capas que van en sentidos contrarios y a distinto
periodo (`vaiven` a 23s, `deriva` a 17s, esta última girando además 12°), lo
que cambia es *dónde se cruzan*, y ahí aparece un tono que no está pintado en
ninguna de las dos. Eso es el tornasol, y es lo que antes daban las manchas
desenfocadas — pero sin repintar nada.

La segunda capa (`.campo.b`) no lleva color de fondo, solo sus radiales, para
que la primera se vea por debajo; la transparencia basta para que al solaparse
den un tercer color, sin `mix-blend-mode`. Como además gira, necesita más
margen que la primera: 160% y -30%, o las esquinas entrarían en pantalla.

Lo único que cambia por cuadro en las dos es su `transform`. Transformar una
capa ya rasterizada es de lo poco que de verdad sale gratis.

Encima van tres capas quietas:

- `#polvo`, un canvas con 140 motas en escritorio y 64 en teléfono que vienen
  del fondo hacia ti. Es lo único que se dibuja por cuadro, y solo mientras el
  hero está a la vista. Da escala: sin algo pequeño y definido, un degradado
  grande no dice a qué distancia está. Cada mota tiene su propio brillo y su
  propio bamboleo; si todas van igual, el conjunto se ve como una rueda
  girando en vez de como polvo. El límite no es pintarlas —son arcos de
  canvas, el dibujo más barato que hay— sino que pasadas unas 150 el campo
  empieza a leerse sucio en vez de profundo.
- `.velo`, un radial oscuro que asienta el texto. Es **más ancho que la
  pantalla** a propósito: una rampa que cambia de pendiente dibuja cantos
  horizontales que se leen como bandas, y así sus bordes caen fuera.
- `.grano`, textura, que de paso rompe el escalonado en franjas que hace un
  degradado oscuro y amplio en pantallas de 8 bits.

**El estado de la cabecera lo lleva un `IntersectionObserver`**, no un bucle.
Antes se recalculaba en cada cuadro aunque no hubiera nada que cambiar. La
cabecera va clara mientras quede hero debajo de ella (`rootMargin` de -90px) y
se posa en cuanto el hero sale.

**En el teléfono la cabecera posada es color sólido, sin `backdrop-filter`.**
Desenfocar lo que hay detrás de una barra fija se recalcula en cada cuadro de
scroll, en toda la pantalla, y es de las cosas más caras que puede hacer una
página en un teléfono.

**La atmósfera del resto de la página** (`.atmos`) siguió el mismo camino: era
un elemento fijo con tres círculos de 62vw desenfocados a 70px. Estáticos, pero
una capa fija y desenfocada detrás de una página que se desplaza se recompone
en cada cuadro. Ahora son tres degradados radiales en el propio `.atmos`,
mismos colores y mismas posiciones, con `isolation:isolate` para que la mezcla
del grano se quede dentro.

**El atajo "Saltar al calendario" se quitó**, del teléfono y del escritorio.
En el teléfono caía justo debajo del botón de WhatsApp, y en los dos sitios
competía con el botón del titular. Las salidas del hero son ahora "Elegir lo
que veo", el "Agendar cita" de la cabecera y el botón de WhatsApp. Debajo del
titular queda solo "Desliza".

**La marca del menú es siempre texto, nunca `logo.png`.** Ese archivo es navy
sobre blanco **opaco**: sobre fondo oscuro no hay filtro que lo salve, porque
cualquier inversión pinta la caja entera. Y alternar imagen y texto al pasar el
hero hacía parpadear la cabecera. El texto solo cambia de color según el fondo.
Si algún día llega el logo en SVG o PNG transparente, aquí es donde entra.

**El menú de los seis síntomas** vive en la banda `.entradas`, a la que apunta
el botón del hero. Es la primera decisión que se le pide al visitante y por eso
no está escondida detrás de nada.

## Concepto

Lo que el visitante elige en la banda `.entradas` es un **síntoma**, no un motivo: el motivo depende de si la consulta es para un
niño o para un adulto, así que el mapeo (`SINTOMAS`) se resuelve en el módulo de
agendar, cuando ya se sabe para quién es.

La idea de las dos pieles —una doctora, dos consultas— vive en los dos carriles
de padecimientos, que es donde es una observación útil y no un acertijo.

En esa banda hay una línea viva, "Puedes pedir cita desde el viernes
27", calculada de la misma constante `HORARIOS`. Dice "pedir", no "reservar",
porque eso es lo que de verdad hace el sitio.

## Secciones ocultas

- **"La consulta" y sus seis pasos.** Retirada a petición del cliente. No está
  borrada: vive entera y comentada en `index.html`, justo antes de la sección
  de agendar. Para recuperarla basta quitar el comentario y devolver sus ligas
  al menú y al pie.

## Caminos ya explorados

Para no volver a recorrerlos sin querer:

- **Hero de recorrido en color** (el scroll pasaba por paradas y el fondo
  cambiaba de tono en cada una; manchas desenfocadas, fondo tornasol, paleta
  morada). Gustó mucho de ver, pero se trababa en Chrome y en teléfono: lo caro
  era el campo de manchas con `filter:blur()` y `mix-blend-mode` repintado en
  cada cuadro. Completo en el historial hasta `54731fc`. Si se retoma, el fondo
  tiene que estar ya rasterizado y lo único que cambie debe ser una
  transformación.
- **Hero de recorrido por la piel** (el scroll metía la cámara cuatro
  milímetros dentro del tejido, con una malla de nodos y una anomalía por
  parada, y un medidor de profundidad en milímetros de verdad). Se sustituyó
  porque al cliente no le convenció. En el historial hasta `a43306e` la versión
  lateral y después la de profundidad.
- **Hero de "las dos pieles"** (titular en dos líneas que se desteñían con el
  cursor, retrato recortado a la derecha, control deslizante). Descartado: era
  la plantilla de medio gremio médico, pedía hover para dar algo y en reposo era
  un póster. Vive en el historial hasta `bb38cb0`.
- **Hero de la pregunta** ("¿Qué le ves a la piel?", centrado y tipográfico, con
  las seis entradas en rejilla). No se descartó por malo: el cliente pidió algo
  como drailse.com y entró el recorrido. Sobrevive como la banda `.entradas`,
  que hoy es la primera decisión que se le pide al visitante y a la que apunta
  el botón del hero. Completo en el historial hasta `52e14f5`.
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
- Instagram: `@dra.silviavillaman` (en el pie y en el `sameAs` del JSON-LD).

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
- **Opiniones.** Se muestran las cuatro de cinco estrellas, textuales, con
  nombre, fecha y estrellas, tal como aparecen en Doctoralia. La quinta es una
  queja por una cita confirmada que no se respetó y no está en la lista, pero el
  pie dice "4 de 5 estrellas en 5 opiniones" y liga al perfil completo: eso es
  lo que evita que se lea como una selección a modo. Lo que atiende esa queja
  es la línea de "agendar": *"la cita queda en firme cuando yo te contesto"*.
  Antes vivía también en el paso 03 de "la consulta", que ahora está oculta,
  así que **esa línea es la única que lo dice: no quitarla.**
  Vale confirmar con la doctora que esté de acuerdo con citarlas en el sitio.

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

## Una trampa del CSS que ya costó una vez

Las consultas de medios van en cascada y hay una, `(min-width:621px) and
(max-width:900px)`, colocada **después** de la de `max-width:900px` a propósito,
para ganarle en el rango de tableta. Solo debe contener `.ops`.

Si se le agrega cualquier otra regla, esa regla deja de aplicar por debajo de
621px. Así se rompió el móvil una vez: `.booking{grid-template-columns:1fr}`
cayó dentro de ese bloque y en teléfono el calendario y el formulario volvieron
a dos columnas encimadas. El aviso está escrito también en el propio CSS.

## Alturas del hero: cuál unidad va en cada sitio

`.viaje` lleva `100svh` y después `100dvh`, en ese orden. El `svh` es el
respaldo para navegadores que no entienden `dvh`; el que manda es `dvh`,
porque tiene que medir lo que mide la pantalla *ahora mismo*. En el teléfono
la barra del navegador se encoge al primer scroll y el viewport crece unos
100 px: con `svh` a secas el hero se quedaba corto y por abajo asomaba la
sección siguiente.

Esto vale porque el hero es **una** pantalla y no se mueve. Cuando era un
recorrido de varias pantallas, el alto total tenía que ir en `vh` —que no
cambia— o el documento entero cambiaba de alto a media lectura y el scroll
daba un salto. Si alguna vez vuelve a crecer, esa distinción vuelve con él.

## Por qué morado y no el espectro completo

Se probaron tres caminos antes de este:

- **Tres familias** (malva, champán, nácar), cada una solo aclarada u
  oscurecida: el cambio no se notaba.
- **El espectro completo** (morado, verde, amarillo, coral): se notaba, pero
  seis tonos sin relación entre sí no dejan ningún color asociado a ella, el
  verde y el amarillo cargan lectura clínica, y no compartía nada con el navy
  del logo.
- **Un arco** (violeta, lavanda, azul, verde azulado, salvia, más un cálido al
  final): mejor, pero el cliente pidió quedarse en morados.

Los seis radiales de `.campo` **no** son el mismo morado aclarado: son vecinos
distintos, de `#5B46C2` a `#9A7BF0`. Donde se solapan aparece un tercer tono
que no está pintado en ningún sitio, y eso es lo que da el tornasol. Si se
igualan, el fondo se vuelve plano.

## Lo que no se puede subir sin mirar el contraste

El texto es casi blanco (`#F4F1FA`) y el párrafo `#CDC6E0`; el acento es
`#C3B0FF`. Ninguno cambia, que es lo que permite mover los tonos del fondo sin
revisar seis contrastes cada vez.

Medido en el peor caso posible —el radial más claro de la primera capa con el
más claro de la segunda encima y el velo al 58 %— sale `rgb(79,62,127)`, y
contra él: título **8.08**, párrafo **5.48**, acento **4.71**. Contra la base
oscura son 15.94, 10.80 y 9.30.

Es un peor caso conservador: esos dos radiales están en esquinas opuestas y
nunca llegan a solaparse del todo. Aun así, **al añadir la segunda capa el
párrafo bajó de 5.77 a 5.48**, y el margen sobre el 4.5 que exige WCAG ya no
es grande. El acento está en 4.71, pero es texto grande, así que su umbral es
3.0.

Lo que sostiene esos números es el velo, no el campo: sobre `#9A7BF0` desnudo
el párrafo cae a **1.97**. Así que si se sube el brillo de los radiales de
cualquiera de las dos capas, se sube la opacidad de la segunda o se baja la
rampa del velo entre el 54 % y el 76 %, lo primero que deja de leerse es el
párrafo. Queda poco margen: toca medir, no estimar. Ya pasó dos veces: con
el campo claro llegó a 1.76, y al quitar un halo blanco el párrafo dejó de
leerse hasta que se oscureció la tinta secundaria.

## El velo no puede lavar media pantalla

Cuando lo hacía, el fondo se quedaba sin la mitad de abajo y el color no se
leía. Es una cama elíptica detrás del texto, más ancha que la pantalla para
que sus bordes caigan fuera: un degradado cuya pendiente cambia de signo
dibuja cantos horizontales que se ven como bandas, y ese fue el origen de un
"rectángulo oscuro" que costó dos intentos encontrar.

El otro origen del mismo rectángulo fue `will-change:transform,opacity`: le
da al elemento una caja de rasterizado de tamaño fijo, y un `blur` que se sale
de ella aparece **cortado en recto**. No se pone `will-change` sin mirar qué
hay dentro.
