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

## Ejemplos de Escenarios Comunes:

### **Escenario 1: Una nueva receta solamente**
✅ **Un solo commit** - Cambios relacionados y cohesivos
```bash
git add recipes/nueva-receta.html index.html
git commit -m "feat(recipes): añade receta de fabada asturiana"
```

### **Escenario 2: Múltiples recetas nuevas**
❌ **Evitar**: Un commit gigante con 3 recetas
✅ **Mejor**: Tres commits separados
```bash
# Commit 1
git add recipes/fabada.html
git commit -m "feat(recipes): añade receta de fabada asturiana"

# Commit 2  
git add recipes/cocido-madrileno.html
git commit -m "feat(recipes): añade receta de cocido madrileño"

# Commit 3
git add index.html
git commit -m "feat(navigation): actualiza menú con nuevas recetas"
```

### **Escenario 3: Correcciones + nueva receta**
✅ **Dos commits lógicos**
```bash
# Primero las correcciones
git add recipes/paella.html
git commit -m "fix(recipes): corrige tiempo de cocción en paella valenciana"

# Luego la nueva funcionalidad
git add recipes/nueva-receta.html index.html
git commit -m "feat(recipes): añade receta de pulpo a la gallega"
```

### **Escenario 4: Recetas + configuración**
✅ **Commits independientes**
```bash
# Funcionalidad principal
git add recipes/ index.html
git commit -m "feat(recipes): añade 2 recetas tradicionales del norte"

# Configuración separada
git add .github/
git commit -m "chore(config): actualiza configuración de prompts"
```

## Instrucciones para crear el commit:

### **Estrategia de Commits Múltiples:**
**IMPORTANTE**: Cuando hay muchos cambios, es mejor crear **varios commits lógicos** en lugar de uno solo grande. Esto mejora la legibilidad del historial y facilita el mantenimiento.

### **Criterios para dividir commits:**
1. **Por funcionalidad**: Cada nueva receta = commit separado
2. **Por tipo de cambio**: Separar correcciones de nuevas características
3. **Por alcance**: Separar cambios de contenido de cambios de configuración
4. **Por independencia**: Si un cambio puede funcionar solo, merece su propio commit

### **Orden recomendado de commits:**
1. **Primero**: Correcciones y mejoras de archivos existentes
2. **Segundo**: Nuevas recetas individuales (una por commit)
3. **Tercero**: Cambios de navegación/estructura
4. **Último**: Configuración y documentación

1. **Revisar cambios actuales:**
   - Usa `git status` para ver archivos modificados
   - Usa `git diff` para revisar cambios específicos
   - **Evalúa si hay múltiples tipos de cambios que requieren commits separados**

2. **Planificar commits lógicos:**
   - **Si hay UNA nueva receta**: Un solo commit `feat(recipes)`
   - **Si hay MÚLTIPLES recetas**: Un commit por cada receta
   - **Si hay recetas + correcciones**: Commits separados
   - **Si hay recetas + configuración**: Commits independientes

3. **Determinar el tipo de commit:**
   - ¿Es una nueva receta? → `feat(recipes)`
   - ¿Es una corrección? → `fix(recipes)` o `fix(navigation)`
   - ¿Es documentación? → `docs`
   - ¿Es configuración? → `chore`

4. **Escribir descripción clara:**
   - Máximo 50 caracteres para el título
   - Usar imperativo: "add", "fix", "update" 
   - **En español para mantener consistencia cultural del proyecto**
   - Ejemplos: "añade", "corrige", "actualiza", "mejora"

5. **Añadir cuerpo si es necesario:**
   - Explicar el "qué" y el "por qué" en español
   - Incluir detalles específicos de la receta
   - Mencionar aspectos culturales relevantes

6. **Añadir footer si aplica:**
   - Referencias a issues: Closes seguido del número de issue
   - Breaking changes: BREAKING CHANGE seguido de descripción

## Comandos sugeridos:

### **Para un solo cambio lógico:**
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

### **Para múltiples cambios - Commits separados:**
```bash
# 1. Revisar todos los cambios
git status

# 2. Commit para correcciones existentes (si las hay)
git add index.html  # Solo correcciones de navegación
git commit -m "fix(navigation): corrige idioma y título de página principal

- Cambiado lang='en' a lang='es' para consistencia
- Actualizado título a 'Recetas Españolas Tradicionales'"

# 3. Commit para nueva receta
git add recipes/chorizo-espanol.html
git commit -m "feat(recipes): añade receta de chorizo español casero

- Añadida receta tradicional con pimentón de La Vera
- Incluye proceso completo de curado tradicional
- Ingredientes auténticos de la gastronomía española"

# 4. Commit para actualizar navegación
git add index.html  # Solo el enlace nuevo
git commit -m "feat(navigation): añade enlace a chorizo español en menú principal"

# 5. Commit para configuración/prompts
git add .github/prompts/
git commit -m "chore(prompts): añade archivos de automatización para VS Code

- nueva-receta.prompt.md: crear recetas siguiendo patrón
- revisar-estructura.prompt.md: auditar estándares HTML  
- mejorar-contenido-cultural.prompt.md: enriquecer autenticidad
- generar-readme.prompt.md: documentación profesional
- crear-commit.prompt.md: commits estandarizados en español"
```

### **Comandos útiles para commits selectivos:**
```bash
# Añadir archivos específicos
git add archivo1.html archivo2.html

# Añadir partes específicas de un archivo (interactivo)
git add -p archivo.html

# Ver qué archivos están en staging
git status --porcelain

# Ver diff de archivos en staging
git diff --cached
```

## Variables disponibles:
- `${workspaceFolder}` - Ruta del proyecto
- `${input:commitType}` - Tipo de commit (feat, fix, docs, etc.)
- `${input:scope}` - Scope del commit (recipes, navigation, etc.)
- `${input:description}` - Descripción del commit

## 🏆 Mejores Prácticas para Commits Múltiples:

### **Ventajas de commits lógicos separados:**
- ✅ **Historial limpio**: Cada commit tiene un propósito claro
- ✅ **Reversión fácil**: Puedes deshacer cambios específicos sin afectar otros
- ✅ **Revisión de código**: Más fácil revisar cambios pequeños y cohesivos
- ✅ **Debugging**: Localizar problemas es más sencillo
- ✅ **Colaboración**: Otros desarrolladores entienden mejor los cambios

### **Regla de oro:**
> "Un commit debe representar **un cambio lógico completo** que por sí solo añade valor al proyecto"

### **Preguntas para evaluar si dividir:**
- ¿Este commit mezcla correcciones con nuevas características?
- ¿Si revierto este commit, se rompe funcionalidad que debería seguir funcionando?
- ¿El mensaje del commit necesita explicar múltiples cosas diferentes?
- ¿Otro desarrollador podría querer aplicar solo parte de estos cambios?

**Si respondes "SÍ" a cualquiera, considera dividir en commits separados.**

¡Crea commits profesionales que reflejen cambios lógicos y cohesivos en tu proyecto de recetas españolas!