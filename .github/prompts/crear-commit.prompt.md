---
mode: 'agent'
description: 'Crear commit siguiendo el estándar Conventional Commits para el proyecto de recetas'
---

# Crear Commit con Conventional Commits

Genera un commit message siguiendo el estándar Conventional Commits para el proyecto Odin Recipes.

**IMPORTANTE**: Este proyecto mantiene consistencia cultural española, por lo que los mensajes de commit deben estar **en español** tanto en el título como en el cuerpo del mensaje.

## Formato de Conventional Commits:
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Tipos de commit para este proyecto:

### Tipos principales:
- **feat**: Nueva receta o funcionalidad
- **fix**: Corrección de errores en recetas o HTML
- **docs**: Cambios en documentación (README, instrucciones)
- **style**: Formato de código, espacios en blanco (no afecta funcionalidad)
- **refactor**: Reorganización de código sin cambiar funcionalidad
- **test**: Añadir o corregir tests (si aplica)
- **chore**: Tareas de mantenimiento, configuración

### Scopes específicos para recetas:
- **recipes**: Cambios en archivos de recetas
- **navigation**: Cambios en index.html o navegación
- **content**: Mejoras de contenido cultural
- **structure**: Cambios en estructura HTML
- **prompts**: Archivos de solicitud de Copilot

## Ejemplos para este proyecto:

### Nuevas recetas:
```
feat(recipes): añade receta de chorizo español casero

- Añadida receta tradicional española con ingredientes auténticos
- Incluye pimentón de La Vera y proceso de curado tradicional
- Actualizada navegación en index.html
```

### Correcciones:
```
fix(recipes): corrige proporciones de ingredientes en paella

- Corregida proporción arroz-caldo en paella valenciana
- Actualizado tiempo de cocción para mejor socarrat
- Corregida lista de ingredientes tradicionales
```

### Documentación:
```
docs: actualiza README con estructura del proyecto

- Añadida documentación completa del proyecto
- Incluidas guías de desarrollo
- Añadido contexto cultural para recetas españolas
```

### Mejoras de contenido:
```
feat(content): mejora autenticidad cultural en recetas

- Añadido contexto regional a tortilla española
- Incluidos consejos de cocina tradicional
- Actualizadas fuentes de ingredientes y alternativas
```

### Configuración:
```
chore(prompts): añade archivo prompt para commits convencionales

- Creado prompt para mensajes de commit estandarizados
- Sigue especificación Conventional Commits
- Adaptado para proyecto de recetas españolas
```

## Instrucciones para crear el commit:

1. **Revisar cambios actuales:**
   - Usa `git status` para ver archivos modificados
   - Usa `git diff` para revisar cambios específicos

2. **Determinar el tipo de commit:**
   - ¿Es una nueva receta? → `feat(recipes)`
   - ¿Es una corrección? → `fix(recipes)` o `fix(navigation)`
   - ¿Es documentación? → `docs`
   - ¿Es configuración? → `chore`

3. **Escribir descripción clara:**
   - Máximo 50 caracteres para el título
   - Usar imperativo: "add", "fix", "update" 
   - **En español para mantener consistencia cultural del proyecto**
   - Ejemplos: "añade", "corrige", "actualiza", "mejora"

4. **Añadir cuerpo si es necesario:**
   - Explicar el "qué" y el "por qué" en español
   - Incluir detalles específicos de la receta
   - Mencionar aspectos culturales relevantes

5. **Añadir footer si aplica:**
   - Referencias a issues: Closes seguido del número de issue
   - Breaking changes: BREAKING CHANGE seguido de descripción

## Comandos sugeridos:

```bash
# Revisar cambios
git status
git diff

# Añadir archivos
git add .

# Crear commit con mensaje
git commit -m "feat(recipes): añade receta de chorizo español casero

- Añadida receta tradicional española con ingredientes auténticos
- Incluye pimentón de La Vera y proceso de curado tradicional  
- Actualizada navegación en index.html"
```

## Variables disponibles:
- `${workspaceFolder}` - Ruta del proyecto
- `${input:commitType}` - Tipo de commit (feat, fix, docs, etc.)
- `${input:scope}` - Scope del commit (recipes, navigation, etc.)
- `${input:description}` - Descripción del commit

¡Crea un commit profesional que refleje los cambios realizados en el proyecto de recetas españolas!