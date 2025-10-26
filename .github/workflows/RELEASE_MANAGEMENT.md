# 🚀 Sistema de Release Management Automático

> **Configuración completa de Release Please para versionado automático y generación de changelogs profesionales**

## 📋 Índice
1. [Overview del Sistema](#overview-del-sistema)
2. [Configuración Implementada](#configuración-implementada)
3. [Flujo de Trabajo](#flujo-de-trabajo)
4. [Conventional Commits](#conventional-commits)
5. [Herramientas Disponibles](#herramientas-disponibles)
6. [Uso Práctico](#uso-práctico)

---

## 🎯 Overview del Sistema

### ✅ **¿Qué se ha Implementado?**

- **🤖 Release Please**: Automatización completa de releases
- **📝 Changelog Personalizado**: Formato atractivo con emojis y secciones
- **🏷️ Versionado Semántico**: Automático basado en commits
- **📋 GitHub Releases**: Notas enriquecidas automáticas
- **🔄 CI/CD Integration**: GitHub Actions configuradas
- **🎭 Chat Modes Especializados**: Para gestión de releases
- **📄 Prompts Reutilizables**: Para tareas comunes

### 🎨 **Personalización para Recetas Españolas**

- **Emojis temáticos**: 🍽️ para recetas, 🔧 para correcciones
- **Secciones específicas**: Organizadas por tipo de contenido
- **Estadísticas**: Contador de recetas por región
- **Enlaces útiles**: Sitio web, issues, discussions
- **Contexto cultural**: Enfocado en gastronomía española

---

## ⚙️ Configuración Implementada

### 📁 **Archivos Creados**

```
.github/
├── workflows/
│   └── release-please.yml         # GitHub Actions workflow
├── chatmodes/
│   └── release-manager.chatmode.md # Chat mode especializado
├── prompts/
│   ├── manage-releases.prompt.md   # Gestión de releases
│   └── create-changelog.prompt.md  # Creación de changelogs
├── release-please-config.json     # Configuración Release Please
└── release-please-manifest.json   # Manifest de versiones

CHANGELOG.md                       # Changelog personalizado
```

### 🔧 **Configuración de Release Please**

```json
{
  "packages": {
    ".": {
      "release-type": "simple",
      "component": "odin-recipes",
      "include-v-in-tag": true,
      "changelog-sections": [
        { "type": "feat", "section": "🍽️ Nuevas Recetas" },
        { "type": "fix", "section": "🔧 Correcciones" },
        { "type": "docs", "section": "📚 Documentación" },
        { "type": "style", "section": "💄 Mejoras de Estilo" }
      ]
    }
  }
}
```

---

## 🔄 Flujo de Trabajo

### 1. **Desarrollo Normal**

```bash
# 1. Crear rama para nueva funcionalidad
git checkout -b feat/nueva-receta-paella

# 2. Hacer cambios y commits con formato convencional
git add recipes/paella-mixta.html
git commit -m "feat: añadir receta de paella mixta valenciana

Agregar receta tradicional de Valencia con:
- Ingredientes auténticos (pollo, conejo, judías verdes)
- Técnicas tradicionales de socarrat
- Consejos culturales y variantes locales"

# 3. Push y crear PR
git push origin feat/nueva-receta-paella
```

### 2. **Merge a Main**

```bash
# Al hacer merge a main, Release Please automáticamente:
# ✅ Analiza los commits desde la última release
# ✅ Calcula la nueva versión (patch/minor/major)
# ✅ Crea un Release PR con changelog
# ✅ Mantiene el Release PR actualizado con nuevos merges
```

### 3. **Crear Release**

```bash
# Cuando estés listo para release:
# ✅ Revisar el Release PR generado
# ✅ Verificar changelog y versión
# ✅ Hacer merge del Release PR
# ✅ Automáticamente se crea tag y GitHub Release
```

---

## 📝 Conventional Commits

### **Formato Base**
```
<tipo>[scope opcional]: <descripción>

[cuerpo opcional]

[pie(s) opcional(es)]
```

### **Tipos Principales**

| Tipo | Incremento | Uso en Recetas | Ejemplo |
|------|------------|----------------|---------|
| `feat` | **MINOR** | Nueva receta | `feat: añadir receta de gazpacho andaluz` |
| `fix` | **PATCH** | Corrección | `fix: corregir ingredientes de tortilla` |
| `docs` | **PATCH** | Documentación | `docs: actualizar README con nuevas recetas` |
| `style` | **PATCH** | Estilo/formato | `style: mejorar espaciado en listas` |
| `refactor` | **PATCH** | Refactoring | `refactor: reorganizar estructura de archivos` |
| `perf` | **PATCH** | Performance | `perf: optimizar imágenes de recetas` |

### **Breaking Changes**
```bash
# Para cambios que rompen compatibilidad (MAJOR)
feat!: reestructurar organización de recetas

BREAKING CHANGE: Las URLs de recetas cambian de /recetas/ a /es/cocina/
```

### **Scopes Sugeridos**
```bash
feat(recetas): añadir nueva receta
fix(navegacion): corregir enlaces rotos
docs(contributing): actualizar guía de contribución
style(index): mejorar diseño de página principal
```

---

## 🛠️ Herramientas Disponibles

### 1. **Chat Mode: Release Manager**

```bash
# Activar modo especializado
@release-manager

# Comandos útiles
"¿Necesitamos una nueva release?"
"¿Qué tipo de versión sería la próxima?"
"Revisa el Release PR actual"
"¿Los commits recientes están bien formateados?"
```

### 2. **Prompt: Gestión de Releases**

```bash
@manage-releases

# Funciones:
# ✅ Verificar estado de release
# ✅ Validar formato de commits
# ✅ Preparar release manual
# ✅ Solucionar problemas
```

### 3. **Prompt: Crear Changelog**

```bash
@create-changelog

# Funciones:
# ✅ Generar changelog personalizado
# ✅ Mejorar formato existente
# ✅ Añadir secciones especiales
# ✅ Optimizar para diferentes audiencias
```

---

## 🎯 Uso Práctico

### **Escenario 1: Nueva Receta**

```bash
# 1. Crear commit convencional
git commit -m "feat: añadir receta de churros con chocolate

Receta tradicional madrileña con:
- Masa de churros casera
- Chocolate espeso tradicional  
- Técnicas de fritura perfecta
- Historia y origen cultural"

# 2. Push a main
git push origin main

# 3. Release Please automáticamente:
# → Detecta nueva funcionalidad (feat)
# → Planifica incremento MINOR (ej: 1.1.0 → 1.2.0)
# → Crea/actualiza Release PR con entrada en changelog
```

### **Escenario 2: Corrección de Bug**

```bash
# 1. Commit de corrección
git commit -m "fix(gazpacho): corregir proporción de aceite de oliva

- Cambiar de 100ml a 75ml para mejor equilibrio
- Actualizar consejos de preparación
- Mejorar descripción de textura final"

# 2. Release Please:
# → Detecta corrección (fix)
# → Planifica incremento PATCH (ej: 1.2.0 → 1.2.1)
# → Actualiza Release PR existente
```

### **Escenario 3: Forzar Versión Específica**

```bash
# Para casos especiales (ej: versión navideña)
git commit --allow-empty -m "chore: release versión navideña 2.0.0" -m "Release-As: 2.0.0"

# Release Please:
# → Ignora cálculo automático
# → Crea release con versión especificada
# → Útil para releases temáticas o milestones
```

### **Escenario 4: Multiple Changes**

```bash
# Commit con múltiples cambios en footers
git commit -m "feat: añadir sección de postres tradicionales

Esta actualización incluye una nueva sección dedicada a postres.

feat(postres): añadir receta de torrijas
fix(navegacion): mejorar enlaces entre secciones  
docs(readme): actualizar índice de contenidos

BREAKING-CHANGE: Cambio en estructura de navegación principal"

# Release Please:
# → Detecta breaking change (MAJOR)
# → Parsea múltiples cambios en footers
# → Genera entradas separadas en changelog
```

---

## 📊 **Changelog Generado**

### **Ejemplo de Resultado**

```markdown
## [1.3.0](https://github.com/jorge-maikel-sierra/odin-recipes/compare/v1.2.0...v1.3.0) (2025-10-25)

### 🍽️ Nuevas Recetas

* añadir receta de churros con chocolate ([a1b2c3d](https://github.com/jorge-maikel-sierra/odin-recipes/commit/a1b2c3d))
* **postres**: agregar torrijas tradicionales de Semana Santa ([e4f5g6h](https://github.com/jorge-maikel-sierra/odin-recipes/commit/e4f5g6h))

### 🔧 Correcciones

* **gazpacho**: corregir proporción de aceite de oliva ([i7j8k9l](https://github.com/jorge-maikel-sierra/odin-recipes/commit/i7j8k9l))
* corregir enlaces de navegación en index ([m1n2o3p](https://github.com/jorge-maikel-sierra/odin-recipes/commit/m1n2o3p))

### 📚 Documentación

* actualizar README con sección de postres ([q4r5s6t](https://github.com/jorge-maikel-sierra/odin-recipes/commit/q4r5s6t))
```

---

## 🚨 **Solución de Problemas**

### **Release Please no crea PR**

```bash
# 1. Verificar commits releasables
git log --oneline --grep="^(feat|fix):" $(git describe --tags --abbrev=0)..HEAD

# 2. Si no hay commits → Necesitas al menos un feat/fix
# 3. Si hay commits → Añadir label release-please:force-run al último commit
# 4. Verificar que no hay PR existente con autorelease: pending
```

### **Versión Incorrecta**

```bash
# Forzar versión específica
git commit --allow-empty -m "chore: release 2.0.0" -m "Release-As: 2.0.0"

# O editar Release PR body:
# BEGIN_COMMIT_OVERRIDE
# feat: descripción corregida del cambio
# END_COMMIT_OVERRIDE
```

### **Changelog Incorrecto**

```bash
# Editar manualmente CHANGELOG.md
# Release Please respetará cambios manuales en próximas actualizaciones
```

---

## ✅ **Beneficios del Sistema**

### **Para Desarrolladores**
- ✅ **Versionado automático** sin pensar en números
- ✅ **Changelog generado** automáticamente 
- ✅ **Commits organizados** con formato estándar
- ✅ **Releases profesionales** sin esfuerzo manual

### **Para el Proyecto**
- ✅ **Historial claro** de todos los cambios
- ✅ **Documentación automática** siempre actualizada
- ✅ **Proceso estandarizado** y predecible
- ✅ **Integración GitHub** completa

### **Para Usuarios**
- ✅ **Notas de release** informativas y atractivas
- ✅ **Navegación fácil** por versiones
- ✅ **Enlaces útiles** a documentación y soporte
- ✅ **Contexto cultural** y educativo

---

## 🎉 **¡Sistema Listo!**

El sistema de Release Management está completamente configurado y listo para usar. Simplemente:

1. **Haz commits** siguiendo Conventional Commits
2. **Push a main** y Release Please trabajará automáticamente
3. **Revisa y merge** el Release PR cuando estés listo
4. **¡Disfruta** de releases automáticas y profesionales!

---

<div align="center">

**🚀 Configurado con Release Please + GitHub Actions + Conventional Commits**

![Automated](https://img.shields.io/badge/Releases-Automated-green?style=for-the-badge)
![Semantic](https://img.shields.io/badge/Versioning-Semantic-blue?style=for-the-badge)
![Conventional](https://img.shields.io/badge/Commits-Conventional-orange?style=for-the-badge)

</div>