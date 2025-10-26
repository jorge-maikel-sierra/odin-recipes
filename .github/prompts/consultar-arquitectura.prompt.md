# Consulta de Arquitectura con @workspace

Usa el modo @workspace para decisiones arquitectónicas del proyecto Odin Recipes.

## Contexto del Proyecto:
- Proyecto de aprendizaje de The Odin Project
- Sitio web estático de recetas españolas tradicionales
- Actualmente: HTML puro sin dependencias
- Estructura simple: index.html + carpeta recipes/
- Sistema de prompts automatizado para desarrollo

## Casos de Uso para @workspace:

### 🏗️ **Decisiones de Escalabilidad**
```
@workspace Tengo [X] recetas españolas y planeo llegar a 50+. ¿Cómo debo reorganizar la estructura para mantenerlo manageable?
```

**Preguntas específicas:**
- ¿Subcarpetas por región? (andaluza/, vasca/, catalana/)
- ¿Organización por tipo? (platos-principales/, postres/, tapas/)
- ¿Sistema de índices automáticos?

### 📁 **Organización de Assets**
```
@workspace ¿Cuál es la mejor estructura para organizar imágenes, futuros estilos CSS, y posibles scripts para un sitio de recetas en crecimiento?
```

**Consideraciones:**
- Optimización de imágenes de recetas
- CDN vs almacenamiento local
- Organización para diferentes tamaños de imagen

### 🔄 **Workflow de Desarrollo**
```
@workspace Revisa mi sistema de prompts actual. ¿Qué mejoras arquitectónicas recomiendas para automatizar mejor el desarrollo de recetas?
```

**Archivos a revisar:**
- `.github/prompts/nueva-receta.prompt.md`
- `.github/prompts/crear-commit.prompt.md`
- `.github/copilot-instructions.md`

### 🎯 **Evolución Tecnológica**
```
@workspace Mi proyecto crece. ¿Cuándo y cómo debo evolucionar de HTML estático a tecnologías más avanzadas sin perder la simplicidad?
```

**Opciones a evaluar:**
- Generadores estáticos (Jekyll, Hugo, 11ty)
- Frameworks ligeros (Astro, SvelteKit)
- Mantenimiento de la filosofía "The Odin Project"

### 🌐 **Integración y Deploy**
```
@workspace ¿Cuál es la mejor estrategia de despliegue y CI/CD para este proyecto de recetas españolas?
```

**Plataformas a considerar:**
- GitHub Pages
- Netlify
- Vercel
- Azure Static Web Apps

## Template de Consulta:

### Para decisiones arquitectónicas:
```
@workspace 

**Contexto**: Proyecto Odin Recipes - [descripción específica]
**Situación actual**: [estado del proyecto]
**Objetivo**: [qué quiero lograr]
**Restricciones**: [limitaciones técnicas o de aprendizaje]

¿Cuál es tu recomendación arquitectónica?
```

### Para revisión de estructura:
```
@workspace 

Revisa la estructura actual de mi proyecto de recetas españolas:

```
[tree de archivos]
```

¿Qué mejoras arquitectónicas recomiendas para:
1. Escalabilidad a 50+ recetas
2. Mantenimiento a largo plazo  
3. Performance y SEO
4. Experiencia de desarrollo

```

## Integración con Prompts Existentes:

### 1. **Decisión → Implementación → Commit**
```
@workspace → revisar-estructura.prompt.md → crear-commit.prompt.md
```

### 2. **Arquitectura → Nueva Receta → Web Review**
```
@workspace → nueva-receta.prompt.md → @web → crear-commit.prompt.md
```

### 3. **Evolución Continua**
```
@workspace (mensual) → updates a prompts → documentación
```

## Variables de Contexto:
- Número de recetas actuales: ${currentRecipeCount}
- Tipo de decisión: ${decisionType}
- Alcance de cambio: ${changeScope}
- Restricciones: ${constraints}