# Revisión Web Especializada

Usa el modo @web para revisar aspectos técnicos del proyecto Odin Recipes.

## Contexto del Proyecto:
- Sitio web estático de recetas españolas tradicionales
- HTML puro sin frameworks
- Enfoque en semántica HTML5 y accesibilidad
- Marcado Schema.org para recetas
- Navegación simple entre páginas

## Áreas de Revisión Prioritarias:

### 🔍 **SEO y Metadatos**
- Meta descriptions específicas para cada receta
- Open Graph tags para compartir en redes sociales
- Structured data correctos para Google Recipes
- URLs amigables y jerarquía de sitio

### ♿ **Accesibilidad Web**
- ARIA labels y roles apropiados
- Contraste de colores (cuando se añada CSS)
- Navegación por teclado
- Compatibilidad con lectores de pantalla
- Alt text descriptivos para imágenes de recetas

### 📱 **Responsive Design**
- Viewport meta tag
- Preparación para diseño móvil
- Estructura flexible para diferentes pantallas

### ⚡ **Performance Web**
- Optimización de carga de imágenes
- Estructura HTML eficiente
- Preparación para lazy loading

### 🌐 **Standards Web**
- Validación HTML5
- Mejores prácticas de markup semántico
- Cross-browser compatibility

## Preguntas Sugeridas para @web:

### Para nuevas recetas:
```
@web Revisa esta nueva receta de [nombre]. ¿Cumple con los estándares web modernos? ¿Qué mejoras de SEO y accesibilidad recomiendas?
```

### Para el sitio completo:
```
@web Analiza mi proyecto de recetas españolas. ¿Qué mejoras técnicas debo priorizar para un mejor ranking en Google y experiencia del usuario?
```

### Para evolución del proyecto:
```
@web Mi sitio de recetas está creciendo. ¿Qué tecnologías web debo considerar para añadir búsqueda, filtros por ingredientes, y compartir en redes sociales?
```

## Integración con Workflow Actual:

1. **Después de crear una receta** → `@web` revisa SEO y accesibilidad
2. **Antes de hacer commit** → Usar `crear-commit.prompt.md` para mensaje
3. **Periódicamente** → `@web` audita todo el sitio para mejoras

## Variables Útiles:
- Número total de recetas: ${recipeCount}
- Última receta añadida: ${lastRecipe}
- Tipo de revisión: ${reviewType} (seo, accessibility, performance)