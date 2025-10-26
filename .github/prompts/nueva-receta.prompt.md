---
mode: 'agent'
description: 'Crear una nueva receta española siguiendo el patrón establecido'
---

# Crear Nueva Receta Española

Tu objetivo es crear una nueva receta española siguiendo exactamente el patrón establecido en el proyecto Odin Recipes.

## Instrucciones:

1. **Solicita información si no se proporciona:**
   - Nombre de la receta (en español)
   - Región de origen
   - Breve descripción de la receta

2. **Estructura requerida para cada receta:**
   - Archivo HTML en `/recipes/[nombre-receta].html`
   - Seguir exactamente la estructura de archivos existentes como plantilla
   - Usar las instrucciones en [copilot-instructions.md](../copilot-instructions.md)

3. **Contenido específico:**
   - **Encabezado**: `lang="es"`, viewport responsive, título descriptivo
   - **Imagen**: Ruta `../images/[nombre].jpg` con `width="400"`
   - **Descripción**: 2 párrafos explicativos sobre la receta y su origen
   - **Ingredientes**: Lista `<ul>` con cantidades exactas y ingredientes auténticos
   - **Pasos**: Lista `<ol>` numerada con pasos detallados en `<strong>`
   - **Consejos**: Tips prácticos y culturales
   - **Navegación**: Enlace de vuelta `<a href="../index.html">Home</a>`

4. **Actualizar index.html:**
   - Agregar enlace a la nueva receta en la lista principal
   - Mantener el orden alfabético o por popularidad

5. **Criterios de calidad:**
   - Solo recetas tradicionales españolas auténticas
   - Ingredientes con cantidades específicas
   - Técnicas de cocina tradicionales
   - Información cultural relevante
   - HTML semántico y accesible

## Variables disponibles:
- `${workspaceFolder}` - Ruta del proyecto
- `${input:nombreReceta}` - Nombre de la receta a crear
- `${input:region}` - Región de origen de la receta

¡Asegúrate de mantener la autenticidad cultural y la simplicidad del HTML puro!