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

## El hero: cuatro milímetros hacia adentro

El scroll **mete la cámara en la piel**. Son planos de tejido apilados en
profundidad que vienen hacia ti y te atraviesan, y cada parada es una anomalía
que vive a su profundidad real: 0.02 mm la resequedad, 0.10 el pigmento,
0.30 los nevos, 1 mm la dermis, 2 mm el folículo, 3.5 la raíz del pelo.

Eso hace que el medidor de la esquina **signifique algo**: marca milímetros de
verdad y el número coincide con el que dice la parada, porque sale de la misma
tabla de anclas (`ANCLAS` interpola entre ellas). Cuando el recorrido era
lateral, la profundidad era decorativa.

**Cada anomalía tiene su color**, en la constante `COLOR` del motor: hielo la
resequedad, terroso el pigmento, violeta el nevo, **rojo las ronchas** porque
son inflamación, ámbar el acné, plata la raíz del pelo. El color tiñe el
gráfico, la mira que lo encuadra, la etiqueta de la parada y su punto en el
riel, así que cada parada se distingue de un vistazo y no solo por el texto.

**El relevo del texto está escalonado**: el que sale se va en 300 ms y el que
entra espera 220 antes de aparecer en 420. Si se cruzaran a la vez se verían
encimados; escalonado se lee como un relevo y no como un parpadeo.

**El scroll se ancla en cada parada.** Hay siete `.ancla` invisibles colocadas
en el punto exacto del recorrido donde cada parada queda centrada, con
`scroll-snap-align`. Un movimiento corto regresa a la parada actual y uno
decidido pasa a la siguiente, así que ya no te quedas en los huecos donde no
hay texto. Es `proximity` y no `mandatory`, y **fuera del hero no hay anclas**:
el resto de la página scrollea normal.

Hubo un momento en que `#doctora` también era destino, para que al pasar la
última parada un movimiento te dejara ya en la sección siguiente. Se quitó: el
salto automático al salir del recorrido se sentía brusco. Si se quiere
recuperar, es una regla de una línea — pero se probó y no gustó.

La posición sale de `calc(var(--at) * var(--rango))`, donde `--rango` es la
altura útil del recorrido (420vh en escritorio, 320vh en móvil, porque la
sección mide distinto). Si cambias la altura de `.viaje`, cambia `--rango` o
las anclas dejan de coincidir con las paradas.

La proyección es perspectiva a mano: cada plano se dibuja a `escala = F / z`,
donde `z` sale de su distancia al progreso actual. Se ven unos 12 a la vez; los
de atrás llegan tenues y pequeños, los de adelante crecen hasta pasarte por
encima y desvanecerse. `DELTA` es cuánto se adelanta cada anomalía a su parada:
cuanto más chico, más cerca y más grande llega.

El recorrido tiene **cuatro paradas y un remate**, no seis: resequedad,
pigmento, ronchas y folículo. Se recortó a propósito. Con seis, el visitante
tenía que atravesar cinco pantallas antes de poder hacer nada, y el hero
pasaba de ser lo que convertía a ser el obstáculo. Ahora son 3.8 pantallas en
escritorio y 3.2 en móvil, y hay una **salida visible** —"Saltar al
calendario"— para quien llega con prisa.

El **menú completo de los seis síntomas** vive en la banda `.entradas`, que se
muestra **siempre** debajo del recorrido, no solo como respaldo. Es una
cuestión de accesibilidad: las paradas ocultas salen del árbol de
accesibilidad, así que quien navega con teclado o lector de pantalla nunca
llegaba a ver el camino principal del sitio.

Una parada puede mostrar **dos gráficos**: el folículo es de donde sale el pelo
y donde empieza el acné, y el texto habla de los dos, así que se dibujan ambos
con su color (`data-sintoma` y `data-sintoma2`).

El recorrido **remata** en una última pantalla que invita a agendar, en vez de
terminar en seco.

Por qué no es una copia del túnel, aunque ahora también se entre: el de ellos
es un viaje **por dentro de un tubo**, una sola cavidad recorrida a lo largo,
y dice "nueve metros, ella cuida el último tramo". Este atraviesa **capas
apiladas** hacia abajo y dice "todo esto pasa en cuatro milímetros". Comparten
mecánica, como dos películas comparten el travelling, pero no geometría, ni
métrica, ni mensaje.

<details><summary>Antes de esto, el recorrido era lateral</summary>

La cámara avanzaba en horizontal sobre un trozo de piel y las lesiones iban
apareciendo al costado. Se cambió porque la profundidad quedaba decorativa:
el medidor decía "2 mm de piel" sin que ese número significara nada. Está
en el historial hasta `a43306e`.

</details>

**El registro es de instrumento, no de ilustración.** Fondo oscuro, la piel
dibujada como una malla de nodos y aristas, y las lesiones como anomalías de
ese campo: no se dibuja un lunar, se dibuja lo que un lunar le hace a la malla.

El color base es el **azul marino del propio logo**, no un verde petróleo. Eso
es deliberado: el verde azulado es el terreno de la referencia, y usarlo sería
cruzar la línea que todo este trabajo ha tratado de no cruzar.

Lo técnico, en `index.html`:

- La malla es una retícula de nodos con desplazamiento fijo. Cada anomalía
  deforma los nodos cercanos y los ilumina: `ronchas` y `lunar` los empujan
  hacia afuera, `mancha` los atrae y los apaga, `seca` los desconecta y deja
  la malla rota.
- Se dibuja en **un solo trazo por capa** —todos los segmentos en un `Path2D`
  y un `stroke`—, y los nodos son `fillRect` y no arcos. Medido: **0.19 ms** por
  cuadro para 900 segmentos y 320 nodos, o sea 16.5 ms de margen a 60 fps.
- Dos capas a distinta velocidad y escala hacen la profundidad, más polvo en
  suspensión que deriva solo.
- **El zoom enfoca.** Entre paradas la vista va abierta; al llegar a una se
  cierra sobre ella hasta 1.30×, y se amplía alrededor de la propia anomalía
  para que no se salga de pantalla al crecer. El fondo se acerca menos que el
  frente (0.45 del delta), que es lo que da la sensación de profundidad.
- **Todo respira.** La malla ondula con dos senos desfasados, los nodos de las
  anomalías laten, los halos se expanden y contraen, los pelos se mecen y la
  mira gira despacio. La cámara también respira, un 0.8%.
- El bucle **se apaga** cuando el hero sale de pantalla (`IntersectionObserver`)
  y cuando la pestaña deja de estar visible. Animar un hero que nadie está
  viendo es gastar batería ajena.
- `mide()` pinta un cuadro de inmediato antes de arrancar el bucle: si solo lo
  agendara, el hero se quedaría en blanco hasta el primer cuadro.
- `0 KB` de descarga: todo es generado. Para comparar, drailse baja unos 600 KB
  de three.js solo para su túnel.
- **La marca del menú es siempre texto, nunca `logo.png`.** Ese archivo es navy
  sobre blanco **opaco**: sobre fondo oscuro no hay filtro que lo salve, porque
  cualquier inversión pinta la caja entera. Y alternar imagen y texto al pasar
  el hero hacía parpadear la cabecera. El texto solo cambia de color según el
  fondo: claro sobre el hero, navy cuando la cabecera se posa. De paso, la
  portada ya no descarga `logo.png` (lo sigue usando el aviso de privacidad).
  Si algún día llega el logo en SVG o PNG transparente, aquí es donde entra.
- Contrastes medidos sobre el fondo oscuro: título 14.7:1, cuerpo 8.1:1,
  etiqueta de capa 6.4:1, barra de escala 4.6:1.
- En pantallas angostas la anomalía se va arriba y el texto baja, porque al
  lado no caben. **Ojo:** la niebla tiene su propia versión en móvil; si se
  queda la clara encima del hero oscuro, lava todo y el texto se vuelve
  ilegible.
- Con `prefers-reduced-motion` el recorrido no se muestra y en su lugar aparece
  la banda `.entradas`, que es el hero anterior con las seis tarjetas.

## Concepto

Lo que el visitante elige, tanto en el recorrido como en la banda de respaldo,
es un **síntoma**, no un motivo: el motivo depende de si la consulta es para un
niño o para un adulto, así que el mapeo (`SINTOMAS`) se resuelve en el módulo de
agendar, cuando ya se sabe para quién es.

La idea de las dos pieles —una doctora, dos consultas— vive en los dos carriles
de padecimientos, que es donde es una observación útil y no un acertijo.

En la banda de respaldo hay una línea viva, "Puedes pedir cita desde el viernes
27", calculada de la misma constante `HORARIOS`. Dice "pedir", no "reservar",
porque eso es lo que de verdad hace el sitio.

## Secciones ocultas

- **"La consulta" y sus seis pasos.** Retirada a petición del cliente. No está
  borrada: vive entera y comentada en `index.html`, justo antes de la sección
  de agendar. Para recuperarla basta quitar el comentario y devolver sus ligas
  al menú y al pie.

## Caminos ya explorados

Para no volver a recorrerlos sin querer:

- **Hero de "las dos pieles"** (titular en dos líneas que se desteñían con el
  cursor, retrato recortado a la derecha, control deslizante). Descartado: era
  la plantilla de medio gremio médico, pedía hover para dar algo y en reposo era
  un póster. Vive en el historial hasta `bb38cb0`.
- **Hero de la pregunta** ("¿Qué le ves a la piel?", centrado y tipográfico, con
  las seis entradas en rejilla). No se descartó por malo: el cliente pidió algo
  como drailse.com y entró el recorrido. Sobrevive como la banda `.entradas`,
  que es lo que se ve cuando el visitante tiene `prefers-reduced-motion`.
  Completo en el historial hasta `52e14f5`.
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
