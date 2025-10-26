---
mode: 'agent'
description: 'Gestionar versionado automático y crear changelog con Release Please'
---

# Gestión de Releases y Changelog

Tu objetivo es gestionar el versionado automático del proyecto usando Release Please y crear changelogs atractivos siguiendo Conventional Commits.

## 📋 Información del Sistema

**Release Please** está configurado para:
- ✅ **Versionado automático** basado en Conventional Commits
- ✅ **Changelog automático** con secciones personalizadas
- ✅ **GitHub Releases** con notas enriquecidas
- ✅ **Badges de versión** actualizados automáticamente

## 🎯 Flujo de Trabajo de Releases

### 1. **Preparación para Release**

Antes de crear una nueva versión:

```bash
# Verificar commits desde la última release
git log --oneline $(git describe --tags --abbrev=0)..HEAD

# Verificar que todos los commits siguen Conventional Commits
git log --oneline --grep="^(feat|fix|docs|style|refactor|perf|test|build|ci|chore)"
```

### 2. **Commits que Activan Release**

Los siguientes tipos de commits activan una nueva release:

- **`feat:`** → Nueva funcionalidad (incrementa versión MINOR)
- **`fix:`** → Corrección de bugs (incrementa versión PATCH)
- **`feat!:`** o **`BREAKING CHANGE:`** → Cambio breaking (incrementa versión MAJOR)

### 3. **Estructura de Commits para Recetas**

#### ✅ **Ejemplos de Commits Correctos:**

```bash
# Nueva receta (MINOR)
feat: añadir receta de paella mixta valenciana
feat(recetas): agregar churros con chocolate tradicionales

# Corrección de contenido (PATCH)
fix: corregir ingredientes de la tortilla española
fix(gazpacho): actualizar proporciones de aceite de oliva

# Documentación (PATCH)
docs: actualizar README con nuevas recetas
docs(contribuir): añadir guía para nuevas recetas

# Mejoras visuales (PATCH)
style: mejorar espaciado en lista de ingredientes
style(navegacion): optimizar enlaces entre recetas

# Cambios importantes (MAJOR)
feat!: reestructurar completa organización de recetas
fix!: cambiar formato de URLs de recetas

BREAKING CHANGE: Las URLs de recetas han cambiado de /recetas/nombre.html a /es/recetas/nombre.html
```

#### ❌ **Commits que NO activan release:**

```bash
# Mantenimiento (no visible en changelog)
chore: actualizar .gitignore
chore(deps): actualizar dependencias de desarrollo

# Build/CI (no visible en changelog) 
build: configurar GitHub Actions
ci: añadir workflow de testing
```

## 🚀 **Proceso de Release**

### Automático (Recomendado)

1. **Hacer commits** siguiendo Conventional Commits
2. **Push a main** → Release Please crea PR automáticamente
3. **Revisar Release PR** → Verificar changelog y versión
4. **Merge Release PR** → Se crea tag y GitHub Release automáticamente

### Manual (Forzar versión específica)

```bash
# Commit vacío con versión específica
git commit --allow-empty -m "chore: release 2.0.0" -m "Release-As: 2.0.0"
git push origin main
```

## 📝 **Plantillas de Commits**

### Para Nuevas Recetas

```bash
feat: añadir receta de [NOMBRE_RECETA]

Agregar receta tradicional de [REGIÓN] con:
- Ingredientes auténticos de la región
- Técnicas de cocina tradicionales  
- Consejos culturales y variantes locales
- HTML semántico optimizado para SEO

Receta incluye:
- [NÚMERO] ingredientes principales
- [NÚMERO] pasos detallados
- Tiempo de preparación: [TIEMPO]
- Origen: [REGIÓN/CIUDAD]
```

### Para Correcciones

```bash
fix([receta]): corregir [ASPECTO_ESPECÍFICO]

- Actualizar [INGREDIENTE] con cantidad correcta
- Corregir [TÉCNICA] según tradición [REGIÓN]
- Mejorar [ASPECTO] para mejor comprensión

Fixes #[ISSUE_NUMBER]
```

### Para Mejoras de Documentación

```bash
docs: actualizar [SECCIÓN] con [MEJORA]

- Añadir información sobre [TEMA]
- Mejorar explicación de [CONCEPTO]
- Actualizar enlaces y referencias
- Optimizar para SEO y accesibilidad
```

## 🎨 **Personalización del Changelog**

El changelog se genera automáticamente con las siguientes secciones:

### 🍽️ **Nuevas Recetas** (feat)
- Nuevas recetas añadidas al sitio
- Variantes regionales incorporadas
- Técnicas culinarias implementadas

### 🔧 **Correcciones** (fix)
- Correcciones de ingredientes
- Mejoras en técnicas de cocina
- Fixes de HTML/CSS/estructura

### 📚 **Documentación** (docs)
- Actualizaciones del README
- Mejoras en comentarios del código
- Guías de contribución

### 💄 **Mejoras de Estilo** (style)
- Mejoras visuales del HTML
- Optimizaciones de CSS
- Mejoras de UX/UI

### ♻️ **Refactoring** (refactor)
- Reestructuración de código
- Optimización de estructura
- Mejoras de mantenibilidad

### ⚡ **Optimizaciones** (perf)
- Mejoras de rendimiento
- Optimización de imágenes
- Mejoras de carga

## 🏷️ **Versionado Semántico**

### **MAJOR (X.0.0)** - Cambios Breaking
```bash
feat!: reestructurar organización completa de recetas
BREAKING CHANGE: Cambio en estructura de URLs y navegación
```

### **MINOR (0.X.0)** - Nuevas Funcionalidades
```bash
feat: añadir receta de paella valenciana
feat: implementar navegación por regiones
```

### **PATCH (0.0.X)** - Correcciones y Mejoras
```bash
fix: corregir ingredientes de gazpacho
docs: actualizar README con nuevas recetas
style: mejorar espaciado en listas
```

## 🎯 **Comandos Útiles**

### Verificar Estado de Release

```bash
# Ver último tag
git describe --tags --abbrev=0

# Ver commits desde último release
git log --oneline $(git describe --tags --abbrev=0)..HEAD

# Ver solo commits que activan release
git log --oneline --grep="^(feat|fix):" $(git describe --tags --abbrev=0)..HEAD
```

### Preparar Release Manualmente

```bash
# Forzar release específica
git commit --allow-empty -m "chore: release 1.2.0" -m "Release-As: 1.2.0"

# Release con notas personalizadas
git commit --allow-empty -m "feat: versión especial navideña" -m "Release-As: 2.0.0"
```

### Corregir Release Notes

Si necesitas corregir las notas de un release después del merge, edita el cuerpo del PR merged y añade:

```markdown
BEGIN_COMMIT_OVERRIDE
feat: añadir receta de roscón de reyes

Nueva receta navideña tradicional española con:
- Ingredientes tradicionales de temporada
- Técnicas de amasado y horneado
- Decoración con frutas confitadas
- Consejos para el relleno perfecto
END_COMMIT_OVERRIDE
```

## 🎉 **Ejemplo de Changelog Generado**

```markdown
# Changelog

## [1.2.0](https://github.com/jorge-maikel-sierra/odin-recipes/compare/v1.1.0...v1.2.0) (2025-10-25)

### 🍽️ Nuevas Recetas

* añadir receta de paella mixta valenciana ([abc123](https://github.com/jorge-maikel-sierra/odin-recipes/commit/abc123))
* agregar churros con chocolate tradicionales ([def456](https://github.com/jorge-maikel-sierra/odin-recipes/commit/def456))

### 🔧 Correcciones

* corregir ingredientes de la tortilla española ([ghi789](https://github.com/jorge-maikel-sierra/odin-recipes/commit/ghi789))
* **gazpacho:** actualizar proporciones de aceite de oliva ([jkl012](https://github.com/jorge-maikel-sierra/odin-recipes/commit/jkl012))

### 📚 Documentación

* actualizar README con nuevas recetas ([mno345](https://github.com/jorge-maikel-sierra/odin-recipes/commit/mno345))
* **contribuir:** añadir guía para nuevas recetas ([pqr678](https://github.com/jorge-maikel-sierra/odin-recipes/commit/pqr678))

### 💄 Mejoras de Estilo

* mejorar espaciado en lista de ingredientes ([stu901](https://github.com/jorge-maikel-sierra/odin-recipes/commit/stu901))
```

## ✅ **Checklist Pre-Release**

Antes de hacer merge del Release PR:

- [ ] **Verificar versión** es la correcta según cambios
- [ ] **Revisar changelog** tiene todos los cambios importantes
- [ ] **Comprobar enlaces** en el changelog funcionan
- [ ] **Validar formato** del changelog es correcto
- [ ] **Verificar que no hay commits** pendientes importantes
- [ ] **Confirmar que tests** pasan (si aplica)
- [ ] **Revisar notas de release** son descriptivas

## 🚨 **Solución de Problemas**

### Release Please no crea PR

1. **Verificar commits releasable** desde último tag:
   ```bash
   git log --oneline --grep="^(feat|fix):" $(git describe --tags --abbrev=0)..HEAD
   ```

2. **Verificar no hay PR pending** con label `autorelease: pending`

3. **Forzar re-run** añadiendo label `release-please:force-run` al último commit

### Corregir versión incorrecta

1. **Crear commit** con `Release-As: X.Y.Z`
2. **O editar PR body** con override de commits

### Actualizar changelog manualmente

1. **Editar CHANGELOG.md** directamente
2. **Commit con** `docs: update changelog`
3. **NO afecta** al versionado automático

---

**Resultado**: Versionado automático profesional con changelogs atractivos y releases bien documentadas 🚀📋