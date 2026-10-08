# Cómo se hizo la web de ECLIPSA

ECLIPSA es una marca **inventada**: la creamos para un tutorial y no vende nada. La web está pensada para dos cosas:

1. Que parezca la tienda online de una marca de lujo de verdad.
2. Que herramientas como Higgsfield Ads Studio puedan leerla y sacar solas el "brand kit": logo, colores, tipografías, eslogan, productos con precio, beneficios y público.

Web publicada: https://equipo-rgb.github.io/eclipsa/

Todo está en un solo archivo `index.html`, sin instalar nada: HTML, CSS y un poco de JavaScript. Las fuentes vienen de Google Fonts y las fotos están en la carpeta `img/`.

---

## 1. Las skills que se usaron

Una "skill" es un paquete de instrucciones que le das a la IA (Claude Code, por ejemplo) para que trabaje como un especialista. Aquí se usaron dos.

### Impeccable (diseño de interfaces)

- **Para qué sirve:** convierte a la IA en un director de arte exigente. Revisa jerarquía, tipografía, espaciado, color, contraste y movimiento. Además trae una lista de "vicios de plantilla" que debe evitar (tarjetas iguales con icono, etiquetas pequeñitas encima de cada título, números 01, 02, 03 que no aportan nada...) y un detector que analiza la página y te dice lo que falla.
- **Qué hizo en esta web:** quitar el aspecto de plantilla. La serif (Instrument Serif) pasa a ser la voz principal, las esquinas son casi rectas, desaparecen las tarjetas de beneficios con números y la página pasa a tener un ritmo de revista.
- **Dónde se consigue:** es un proyecto de código abierto (licencia Apache 2.0) que se distribuye como paquete `impeccable` en npm; su detector se lanza con `npx impeccable detect index.html`. Para instalarlo en tu herramienta, busca "Impeccable skill" y sigue las instrucciones oficiales del proyecto.

### emil-design-eng (micro-interacciones y movimiento)

- **Para qué sirve:** recoge la filosofía de Emil Kowalski (diseñador e ingeniero conocido por el detalle de sus animaciones) sobre cuándo animar algo, con qué curva y durante cuánto tiempo. Reglas como: los botones se hunden un poco al pulsarlos, nada aparece de la nada, los efectos al pasar el ratón solo en ordenador y siempre hay que respetar a quien pide "reducir movimiento".
- **Qué hizo en esta web:** el eclipse que avanza al hacer scroll, la entrada suave de cada bloque, el subrayado de cobre del menú, el contador de la cesta que da un pequeño salto y el "+" que gira al añadir un producto.
- **Dónde se consigue:** en GitHub, en el repositorio `emilkowalski/skill`. Con el instalador de skills se añade así:

```bash
npx skills add emilkowalski/skill
```

---

## 2. Prompts para pedir una web así para tu producto

Copia, rellena los [huecos] y pégalo en ChatGPT o Claude. Funcionan mejor en este orden.

### Prompt 1: la web completa

```text
Actúa como director de arte de una marca de lujo y como desarrollador front-end.
Créame la tienda online de una sola página de mi marca [NOMBRE DE LA MARCA], que vende [QUÉ VENDES].

Datos de la marca:
- Eslogan: «[TU ESLOGAN]»
- Productos (nombre, precio en euros y descripción corta):
  1. [PRODUCTO 1], [PRECIO] €, [DESCRIPCIÓN]
  2. [PRODUCTO 2], [PRECIO] €, [DESCRIPCIÓN]
  3. [PRODUCTO 3], [PRECIO] €, [DESCRIPCIÓN]
- Beneficios: [BENEFICIO 1], [BENEFICIO 2], [BENEFICIO 3], [BENEFICIO 4]
- Público: [PARA QUIÉN ES]
- Colores: [COLOR PRINCIPAL] y [COLOR DE ACENTO]
- Fotos: están en la carpeta img/ con estos nombres: [LISTA DE ARCHIVOS]

Secciones: cabecera con logo y cesta, portada con el eslogan como titular, un bloque de manifiesto,
la colección con precios, por qué elegirnos, lookbook con las fotos, para quién es y pie de página.

Reglas:
- Un solo archivo index.html con el CSS y el JS dentro. Sin frameworks ni instalaciones.
- Fuentes de Google Fonts: una serif elegante para titulares y una sans sobria para el texto.
- Nivel de diseño de una marca de lujo real, no una plantilla: mucho aire, esquinas casi rectas,
  nada de tarjetas idénticas con iconos, nada de etiquetas pequeñas encima de cada titular.
- Añade datos estructurados JSON-LD con la organización y cada producto con su precio en EUR.
- Todo el texto en español de España.
- Que se vea perfecta en móvil.
```

### Prompt 2: el movimiento y los detalles

```text
Sobre esta misma web, añade movimiento con sentido, al estilo de Emil Kowalski y de Apple:
- Los bloques entran con suavidad al hacer scroll (opacidad, un pequeño desplazamiento y un desenfoque ligero),
  con curvas de salida fuertes, nada de rebotes.
- Un único momento de firma que sea [EL SÍMBOLO DE TU MARCA, por ejemplo: un eclipse, una ola, un sello],
  animado según avanzas con el scroll.
- Botones que se hunden un 3 % al pulsar; efectos al pasar el ratón solo en ordenador.
- Un contador de cesta que reacciona al añadir un producto.
- Respeta "prefers-reduced-motion": si la persona pide menos movimiento, quita los desplazamientos.
- Sin JavaScript, todo el contenido debe verse igualmente.
Anima solo transform, opacity, filter y clip-path, para que vaya fluido.
```

### Prompt 3: la revisión final

```text
Revisa esta web como un diseñador exigente antes de publicarla y corrige lo que encuentres:
contraste de los textos, tamaños de letra en móvil, textos que se parten mal, elementos que se salen
de la pantalla en un móvil de 390 px, botones desalineados, estados de foco para teclado, color de la
selección de texto y textos alternativos de las imágenes.
Devuélveme el archivo completo corregido y una lista corta de lo que has cambiado.
```

---

## 3. Publicarla gratis en GitHub Pages, paso a paso

1. **Crea una cuenta** gratuita en https://github.com si no la tienes.
2. **Crea un repositorio nuevo:** botón verde "New". Ponle un nombre (por ejemplo `mi-marca`), márcalo como **Public** y pulsa "Create repository".
3. **Sube los archivos:** en el repositorio, pulsa "uploading an existing file" (o "Add file" > "Upload files") y arrastra `index.html`, el logo y la carpeta `img`. Abajo, pulsa "Commit changes".
4. **Activa Pages:** ve a "Settings" > "Pages". En "Build and deployment", elige "Deploy from a branch", rama `main` y carpeta `/ (root)`. Pulsa "Save".
5. **Espera un minuto** y recarga esa misma página de ajustes. Aparecerá tu dirección:
   `https://TU-USUARIO.github.io/mi-marca/`
6. **Para cambiar algo después**, edita el archivo en GitHub (icono del lápiz) o súbelo de nuevo y haz "Commit changes". La web se actualiza sola en un minuto más o menos.

Si usas la terminal, los pasos 3 y 6 son:

```bash
git add .
git commit -m "Actualizo la web"
git push
```

---

*ECLIPSA es una marca de demostración creada para un tutorial. No se venden productos reales.*
