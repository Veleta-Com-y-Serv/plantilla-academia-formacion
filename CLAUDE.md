# Plantilla «Academia y formación» (NoExpertise)

Web estática Astro 5 + Tailwind 4, derivada de AstroWind (MIT, ver `LICENSE-ASTROWIND.md`). Estilo: **fresco**.
Todo en castellano de España, frases cortas, sin jerga.

## Dónde está cada cosa
- **Textos, contacto, menú, SEO y secciones**: `site.yaml` (raíz). Es la única fuente de contenido. `src/data/site.ts` lo carga y lo tipa; no pongas textos fijos en los componentes.
- **Colores**: `site.yaml → colores` (primario = índigo, acento = lima, fondo, texto). `src/layouts/Base.astro` los convierte en variables CSS (`--c-primario`…, y calcula `--c-sobre-primario`/`--c-sobre-acento`, el color de texto legible encima) y Tailwind los expone como `bg-primario`, `text-acento`, `bg-primario/10`… Los derivados (`--gris`, `--indigo-suave`, `--lima-suave`, `--linea`) se calculan con `color-mix` en `src/styles/global.css`.
- **Tipografías y aspecto general**: `src/styles/global.css` (sans del sistema con peso 800 en títulos; botones `.btn` lima, `.btn-sec`, `.btn-indigo`; bandas `.banda-gris` y `.banda-indigo`; `.tarjeta`, `.esquina` (1.25 rem), `.insignia`, `.icono-cuadro`, `.puntos`; cabecera `.cabecera--escuela` y pie `.pie--indigo`). Fuentes del sistema: no añadas fuentes externas.
- **Imágenes**: en `public/`. En `site.yaml` se referencian con ruta absoluta (`/fotos/hero.jpg`). Las fotos de la demo se generan a partir de `fotos.json`; mientras una foto `/fotos/x.jpg` no exista, `Imagen.astro` usa su respaldo `/placeholders/x.svg`. Las fotos nuevas del cliente van en `public/fotos/`.
- **Personas**: `personas[].foto` usa la silueta `/placeholders/persona.svg` salvo que el cliente aporte su foto. No se generan caras.
- **Logo**: `site.yaml → logo` (ruta en `public/`); vacío = sello geométrico + nombre en texto.
- **Pie «Web creada con NoExpertise»**: `site.yaml → marca_pie`.

## Secciones
Cada sección es un componente que solo se pinta si `site.yaml → secciones.<id>.activa` es `true`. Se pintan en el orden en que aparecen en `site.yaml`. `hero` y `contacto` son obligatorias. Fondos alternos: blanco / gris muy claro / índigo (cada componente lleva el suyo).
- `hero` → `src/components/secciones/hero.astro` (blanco, formas geométricas y pegatina)
- `cursos` → `src/components/secciones/cursos.astro` (gris)
- `metodo` → `src/components/secciones/metodo.astro` (índigo)
- `profesores` → `src/components/secciones/profesores.astro` (blanco)
- `resultados` → `src/components/secciones/resultados.astro` (gris)
- `preguntas` → `src/components/secciones/preguntas.astro` (blanco, acordeón)
- `inscripcion` → `src/components/secciones/inscripcion.astro` (índigo, formulario)
- `contacto` → `src/components/secciones/contacto.astro` (gris)
- Diseño extra para apartados nuevos: `texto` (genérico).

## Reglas para órdenes
1. **Activar o quitar una sección**: cambia `activa: true|false` en `site.yaml`. No borres componentes.
2. **Reordenar secciones**: mueve el bloque dentro de `secciones:` en `site.yaml`.
3. **Cambiar textos, teléfonos, horarios, precios, fotos**: solo en `site.yaml`. Si un texto contiene comas o dos puntos dentro de `{ … }`, ponlo entre comillas. **Precios siempre como cadena** (`precio: "75 €/mes"`); precio vacío = no se muestra.
4. **Listas planas**: nunca una lista dentro de un elemento de lista. Si un curso necesita varios valores, usa una cadena (`modalidad: "Presencial u online"`). Máximo 24 elementos por lista.
5. **Página nueva**: crea `src/pages/<slug>.astro` usando `import Base from '../layouts/Base.astro'` y `<Base titulo="…" descripcion="…">…</Base>`; enlázala en `site.yaml → menu` (`{ texto: …, url: "/<slug>" }`). Ejemplo: `src/pages/aviso-legal.astro`.
6. **Apartado nuevo**: añade `secciones.<id-nuevo>` en `site.yaml` con `tipo:` igual al diseño que quieras (`texto`, `cursos`, `metodo`, `preguntas`, `profesores`…). Si ninguno sirve, crea `src/components/secciones/<id>.astro` copiando la estructura de uno existente (`seccion(Astro.props.id)`, `Seccion`, `Titular`). `src/pages/index.astro` lo recoge solo: no lo toques.
7. **No tocar** `src/layouts/Base.astro`, `src/pages/index.astro`, `tailwind.config.mjs` ni `src/styles/comun.css` salvo orden explícita de diseño.
8. Mantén: `lang="es-ES"`, imágenes con `Imagen.astro` (carga diferida), sin JavaScript nuevo salvo necesidad real, 16 px de margen lateral (`.contenedor`), sin scroll horizontal en móvil (las formas geométricas van dentro de secciones con `overflow-hidden`).
9. Los formularios no tienen servidor: abren el correo (`mailto`) con el email de `site.yaml` y llevan los campos en el cuerpo. Para envío real, cambia `action` en `src/components/ui/FormularioContacto.astro` y en `inscripcion.astro`.
10. Nunca inventes precios, cifras, opiniones ni datos que el cliente no haya dado: deja el campo vacío o la lista vacía (los componentes lo ocultan).

## Construir
```
npm ci && npm run build      # resultado en dist/
npx astro preview            # ver dist/ en http://localhost:4321
```
Antes de dar una orden por buena: `npm run build` sin errores.

## Campos propios de esta plantilla (además del esquema común)
- `hero.destacado` {titulo, texto}: pegatina girada sobre la foto («Próximo inicio: octubre»). `hero.items`: 3 etiquetas con `titulo` e `icono`. `hero.cta` y `hero.cta_secundario`.
- `cursos.cursos[]`: `{ titulo, texto, nivel, duracion, precio, inicio, modalidad, imagen, destacado }` (`destacado` true/false: banda y borde lima). Los campos vacíos no se pintan. `cursos.destacado_texto`: etiqueta de los destacados (opcional). `cursos.cta`: botón de cada tarjeta (lleva a la inscripción y deja elegido ese curso).
- `metodo.items`: puntos con `icono`; `metodo.imagen` opcional.
- `profesores.personas[]`. Sin personas, la sección no se pinta.
- `resultados.items`: cifras (`titulo` = la cifra, `texto` = qué mide) + `texto` + `imagen` opcional. **Sin ninguna cifra la sección no se pinta.**
- `preguntas.preguntas[]` en acordeón; `preguntas.cta` opcional.
- `inscripcion`: formulario (nombre, email, teléfono, curso, mensaje). El desplegable «Curso» sale solo con los títulos de `cursos` de todas las secciones activas. `inscripcion.nota`, `inscripcion.items` (ventajas con check), `inscripcion.boton`.
- `contacto.imagen`: plano que enlaza a mapas. `contacto.boton`: texto del botón del formulario.
- Iconos disponibles (`icono:`): los de `src/components/ui/Icono.astro` (p. ej. `usuarios`, `portatil`, `libro`, `birrete`, `globo`, `diana`, `bombilla`, `grafico`, `calendario`, `reloj`, `check`).
