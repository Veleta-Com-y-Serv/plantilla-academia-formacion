# Plantilla NoExpertise · Academia y formación

Web fresca para academias, autoescuelas y centros de formación, con cursos, método e inscripción.

- Sectores: academia, formacion, autoescuela, idiomas, oposiciones. Estilo: fresco. Plan mínimo: presencia.
- Secciones: hero, cursos, metodo, profesores, resultados, preguntas, inscripcion, contacto (más el diseño `texto` para apartados nuevos).
- Campos propios: `cursos[]` (con insignias y `destacado`), `hero.destacado` (pegatina), `resultados.items` (cifras; sin cifras no se pinta), formulario de inscripción con desplegable de cursos.
- Contenido editable en `site.yaml`; metadatos en `template.yaml`; reglas para el agente en `CLAUDE.md`; fotos de la demo en `fotos.json`.

```
npm ci
npm run dev      # desarrollo
npm run build    # web estática en dist/
```

Basada en [AstroWind](https://github.com/onwidget/astrowind) © onWidget, licencia MIT (`LICENSE-ASTROWIND.md`). Imágenes de relleno (`public/placeholders/`) generadas para esta plantilla, sin licencias de terceros.
