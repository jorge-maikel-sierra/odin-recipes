# Odin Recipes

![version](https://img.shields.io/badge/version-1.0.0-blue)

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![The Odin Project](https://img.shields.io/badge/The%20Odin%20Project-F2B632?style=flat-square&logo=theodinproject&logoColor=black)](https://www.theodinproject.com/)
[![Spanish](https://img.shields.io/badge/Language-Spanish-green?style=flat-square)](https://github.com/jorge-maikel-sierra/odin-recipes)

Una colección de recetas tradicionales españolas construida con HTML puro como parte del curriculum de [The Odin Project](https://www.theodinproject.com/). Este proyecto se enfoca en practicar HTML semántico y estructura de documentos web básicos.

[Características](#características) • [Cómo empezar](#cómo-empezar) • [Estructura del proyecto](#estructura-del-proyecto) • [Recetas incluidas](#recetas-incluidas)

## Características

- **HTML Semántico**: Estructura clara y accesible usando elementos HTML5 apropiados
- **Recetas Auténticas**: Tres recetas tradicionales españolas con ingredientes y pasos detallados
- **Navegación Simple**: Enlaces relativos entre páginas sin frameworks complejos
- **Responsive**: Diseño que se adapta a diferentes tamaños de pantalla usando viewport meta tag
- **Sin Dependencias**: HTML puro sin CSS frameworks o JavaScript

## Cómo empezar

### Prerrequisitos

- Un navegador web moderno (Chrome, Firefox, Safari, Edge)
- Ninguna instalación o dependencia adicional requerida

### Ejecutar localmente

1. **Clona el repositorio:**
   ```bash
   git clone https://github.com/jorge-maikel-sierra/odin-recipes.git
   cd odin-recipes
   ```

2. **Abre el archivo principal:**
   ```bash
   # macOS
   open index.html
   
   # Linux
   xdg-open index.html
   
   # Windows
   start index.html
   ```

3. **O simplemente abre `index.html` directamente en tu navegador preferido**

> [!TIP]
> No necesitas un servidor web para este proyecto. Al ser HTML estático puro, funciona perfectamente abriendo los archivos directamente desde el sistema de archivos.

## Estructura del proyecto

```
odin-recipes/
├── index.html              # Página principal con lista de recetas
├── recipes/                # Directorio de recetas individuales
│   ├── paella.html        # Receta de Paella Valenciana
│   ├── tortilla-espanola.html  # Receta de Tortilla Española
│   └── gazpacho.html      # Receta de Gazpacho Andaluz
├── images/                # Imágenes de recetas (referenciadas)
└── README.md              # Este archivo
```

## Recetas incluidas

- **🥘 Paella Valenciana** - El emblemático arroz español con pollo, conejo y verduras
- **🥚 Tortilla Española** - La clásica tortilla de patatas española
- **🍅 Gazpacho Andaluz** - La refrescante sopa fría de tomate tradicional del sur de España

Cada receta incluye:
- Descripción cultural e histórica
- Lista completa de ingredientes con cantidades
- Instrucciones paso a paso detalladas
- Consejos tradicionales de cocina
- Navegación de vuelta a la página principal

## Patrones de desarrollo

Este proyecto sigue convenciones específicas para mantener consistencia:

### Estructura HTML estándar
- `lang="es"` para contenido en español
- Meta viewport para diseño responsive
- Títulos descriptivos específicos por página

### Organización de contenido
```html
<h1>Nombre de la Receta</h1>
<img src="../images/[nombre].jpg" alt="Descripción" width="400">
<h2>Descripción</h2>
<h2>Ingredientes</h2>
<h2>Pasos</h2>
<h2>Consejos</h2>
<a href="../index.html">Home</a>
```

### Convenciones de nombres
- Archivos HTML en minúsculas con guiones
- Imágenes referenciadas como `../images/[nombre].jpg`
- Enlaces relativos para navegación

## Acerca de The Odin Project

Este proyecto es parte del curriculum gratuito de [The Odin Project](https://www.theodinproject.com/), una plataforma de aprendizaje open-source que enseña desarrollo web full-stack. Específicamente, forma parte de las lecciones de HTML básico en la sección de Fundamentos.

### Objetivos de aprendizaje
- Práctica de HTML semántico
- Estructura de archivos y navegación
- Atributos de imagen y enlaces
- Organización de contenido con listas
- Uso apropiado de elementos de encabezado