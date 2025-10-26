# Copilot Instructions - Odin Recipes

## Arquitectura del Proyecto

Este es un sitio web estático de recetas españolas construido con HTML puro, sin frameworks ni dependencias. Es parte del curriculum de [The Odin Project](https://www.theodinproject.com/) enfocado en practicar HTML básico.

### Estructura de Archivos
```
/
├── index.html          # Página principal con lista de recetas
├── recipes/            # Directorio de recetas individuales
│   ├── paella.html
│   ├── tortilla-espanola.html
│   └── gazpacho.html
└── images/            # Imágenes de recetas (referenciadas pero no incluidas)
```

## Patrones y Convenciones Específicas

### Estructura HTML de Recetas
Cada archivo de receta sigue un patrón estricto:
1. **Encabezado estándar**: `lang="es"`, viewport responsive, título descriptivo
2. **Estructura de contenido**:
   - `<h1>` con nombre de la receta
   - `<img>` con ruta relativa `../images/[nombre].jpg` y `width="400"`
   - `<h2>Descripción</h2>` con 2 párrafos explicativos
   - `<h2>Ingredientes</h2>` con lista `<ul>` de ingredientes específicos
   - `<h2>Pasos</h2>` con lista `<ol>` numerada y pasos en `<strong>`
   - `<h2>Consejos</h2>` con tips prácticos
   - Enlace de vuelta: `<a href="../index.html">Home</a>`

### Convenciones de Contenido
- **Idioma**: Todo el contenido está en español, incluidos atributos HTML
- **Recetas auténticas**: Solo recetas tradicionales españolas (paella valenciana, tortilla española, gazpacho andaluz)
- **Detalles específicos**: Ingredientes con cantidades exactas, técnicas tradicionales, consejos culturales
- **Navegación simple**: Solo enlaces relativos entre páginas

## Flujo de Desarrollo

### Agregar Nueva Receta
1. Crear archivo HTML en `/recipes/[nombre-receta].html`
2. Seguir la estructura exacta de archivos existentes
3. Agregar enlace en `index.html` dentro de la lista `<ul>`
4. Asegurar que la imagen se referencie como `../images/[nombre].jpg`

### Comandos de Desarrollo
- **Abrir localmente**: `open index.html` (macOS) o directamente en navegador
- **No hay build process**: Es HTML estático puro
- **No hay dependencias**: No requiere npm, yarn, o package managers

### Estilo de Commits
Usa [Conventional Commits](https://www.conventionalcommits.org/):
- `feat:` para nuevas recetas
- `docs:` para cambios en README
- `fix:` para correcciones de contenido

## Consideraciones Específicas

- **Sin CSS/JavaScript**: Mantener el enfoque en HTML semántico puro
- **Accesibilidad**: Usar `alt` descriptivos en imágenes, estructura de headings apropiada
- **Referencias culturales**: Mantener autenticidad en nombres y técnicas de cocina española
- **Imágenes**: Las rutas están definidas pero las imágenes físicas no están en el repo

Al trabajar en este proyecto, prioriza la simplicidad, la semántica HTML correcta y la preservación de la autenticidad cultural de las recetas españolas.