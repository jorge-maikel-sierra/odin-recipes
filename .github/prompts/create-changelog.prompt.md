---
mode: 'agent'
description: 'Crear y mejorar changelogs atractivos con formato personalizado'
---

# Crear Changelog Atractivo

Tu objetivo es crear y mejorar changelogs visualmente atractivos y informativos siguiendo las mejores prácticas de documentación.

## 🎯 **Configuración del Proyecto**

- **Tipo**: Sitio web estático de recetas españolas tradicionales
- **Audiencia**: Desarrolladores web principiantes y amantes de la cocina española
- **Versionado**: Semantic Versioning (SemVer)
- **Commits**: Conventional Commits
- **Automatización**: Release Please

## 📋 **Estructura Base del Changelog**

### 1. **Header Atractivo**
```markdown
# 📝 Changelog - [NOMBRE_PROYECTO]

> **Breve descripción del propósito del changelog**
> 
> Enlaces a estándares: [Versionado Semántico](https://semver.org/lang/es/) y [Conventional Commits](https://www.conventionalcommits.org/es/v1.0.0/).

---
```

### 2. **Leyenda de Cambios**
```markdown
## 🎯 **Leyenda de Cambios**

| Emoji | Tipo | Descripción |
|-------|------|-------------|
| 🍽️ | `feat` | **Nuevas Recetas** - Descripción específica |
| 🔧 | `fix` | **Correcciones** - Descripción específica |
| 📚 | `docs` | **Documentación** - Descripción específica |
| 💄 | `style` | **Mejoras Visuales** - Descripción específica |
| ♻️ | `refactor` | **Refactoring** - Descripción específica |
| ⚡ | `perf` | **Optimizaciones** - Descripción específica |
```

### 3. **Sección Unreleased**
```markdown
### 🎉 [Unreleased]

> **Próximos cambios** que se incluirán en la siguiente versión

#### 🍽️ Nuevas Recetas
- *Próximamente nuevas recetas tradicionales...*

#### 🔧 Correcciones
- *Mejoras en recetas existentes...*
```

### 4. **Formato de Versiones**
```markdown
## [X.Y.Z](link-to-compare) (YYYY-MM-DD)

### 🍽️ Nuevas Recetas
- feat: añadir receta de paella valenciana ([hash](link-to-commit))
- feat(postres): agregar torrijas tradicionales ([hash](link-to-commit))

### 🔧 Correcciones  
- fix: corregir ingredientes de gazpacho ([hash](link-to-commit))
- fix(tortilla): actualizar tiempo de cocción ([hash](link-to-commit))

### 📚 Documentación
- docs: actualizar README con nuevas recetas ([hash](link-to-commit))
```

## 🎨 **Personalización por Tipo de Proyecto**

### Para Sitio de Recetas Españolas

#### **Emojis Específicos**
```markdown
🍽️ Nuevas Recetas (feat)
🔧 Correcciones (fix) 
📚 Documentación (docs)
💄 Mejoras Visuales (style)
♻️ Refactoring (refactor)
⚡ Optimizaciones (perf)
🧪 Testing (test)
🏗️ Build (build)
🔄 CI/CD (ci)
🧹 Mantenimiento (chore)
```

#### **Secciones Adicionales**
```markdown
### 📊 **Estadísticas del Proyecto**

#### 🍽️ **Recetas por Región**
- **🥘 Valencia**: Paella Valenciana, Paella Mixta
- **🍅 Andalucía**: Gazpacho, Salmorejo
- **🥚 País Vasco**: Tortilla Española

#### 📈 **Evolución del Proyecto**
- **Total de Recetas**: X recetas tradicionales
- **Regiones Representadas**: X comunidades autónomas
- **Técnicas Culinarias**: X+ técnicas tradicionales
```

### Para APIs REST

#### **Emojis API**
```markdown
🚀 Nuevos Endpoints (feat)
🔧 Bug Fixes (fix)
🔒 Seguridad (security)
📈 Performance (perf)
💾 Base de Datos (database)
🔑 Autenticación (auth)
📱 Móvil (mobile)
🌐 Integración (integration)
```

#### **Secciones API**
```markdown
### 🚀 Nuevos Endpoints
- feat: añadir endpoint GET /api/v1/users ([hash](link))
- feat(auth): implementar JWT refresh tokens ([hash](link))

### 🔧 Bug Fixes
- fix: corregir validación en endpoint users ([hash](link))
- fix(auth): resolver problema de expiración de tokens ([hash](link))

### 🔒 Seguridad
- security: actualizar dependencias con vulnerabilidades ([hash](link))
- security(auth): mejorar validación de permisos ([hash](link))

### 📈 Performance
- perf: optimizar consultas de base de datos ([hash](link))
- perf(cache): implementar cache Redis ([hash](link))

### 💾 Base de Datos
- database: añadir índices para mejorar rendimiento ([hash](link))
- database(migration): migración para nueva tabla users ([hash](link))
```

## 🛠️ **Herramientas y Automatización**

### Release Please Configuration

```json
{
  "changelog-sections": [
    {
      "type": "feat",
      "section": "🚀 Nuevas Funcionalidades",
      "hidden": false
    },
    {
      "type": "fix", 
      "section": "🔧 Correcciones",
      "hidden": false
    },
    {
      "type": "docs",
      "section": "📚 Documentación", 
      "hidden": false
    },
    {
      "type": "style",
      "section": "💄 Mejoras de Estilo",
      "hidden": false
    },
    {
      "type": "refactor",
      "section": "♻️ Refactoring",
      "hidden": false
    },
    {
      "type": "perf",
      "section": "⚡ Optimizaciones",
      "hidden": false
    }
  ]
}
```

### GitHub Actions Enhancement

```yaml
- name: Enhance Changelog
  run: |
    VERSION="${{ needs.release-please.outputs.version }}"
    
    # Añadir estadísticas al changelog
    TOTAL_RECIPES=$(find recipes/ -name "*.html" | wc -l)
    TOTAL_COMMITS=$(git rev-list --count HEAD)
    
    # Insertar estadísticas después de la versión
    sed -i "/## \[${VERSION}\]/a\\
    \\
    > 📊 **Estadísticas**: ${TOTAL_RECIPES} recetas totales, ${TOTAL_COMMITS} commits\\
    " CHANGELOG.md
```

## 📝 **Plantillas Específicas**

### Template para Nueva Versión

```markdown
## [${VERSION}](${COMPARE_URL}) (${DATE})

> 📊 **Estadísticas**: ${TOTAL_ITEMS} elementos totales, ${COMMITS_COUNT} commits en esta versión

### 🎉 **Destacados de esta Versión**
- **${MAIN_FEATURE}**: ${DESCRIPTION}
- **Mejoras**: ${IMPROVEMENTS_SUMMARY}
- **Correcciones**: ${FIXES_SUMMARY}

### 🍽️ Nuevas Recetas
${FEAT_ITEMS}

### 🔧 Correcciones
${FIX_ITEMS}

### 📚 Documentación  
${DOCS_ITEMS}

### 💄 Mejoras Visuales
${STYLE_ITEMS}

---
```

### Template para Sección Unreleased

```markdown
### 🎉 [Unreleased]

> **En desarrollo** - Cambios que se incluirán en la próxima versión

#### 🚧 **En Progreso**
- Trabajando en nueva receta de ${UPCOMING_RECIPE}
- Mejorando ${IMPROVEMENT_AREA}
- Optimizando ${OPTIMIZATION_TARGET}

#### 🔜 **Próximamente**
- [ ] Nueva sección de ${UPCOMING_SECTION}
- [ ] Integración con ${UPCOMING_INTEGRATION}
- [ ] Mejoras en ${UPCOMING_IMPROVEMENT}

#### 💡 **Ideas en Consideración**
- Posible implementación de ${POTENTIAL_FEATURE}
- Evaluando ${EVALUATION_ITEM}
- Investigando ${RESEARCH_ITEM}

---
```

## 🎯 **Mejores Prácticas**

### 1. **Consistencia Visual**
- Usar emojis consistentes para cada tipo
- Mantener formato uniforme en todas las entradas
- Agrupar cambios relacionados
- Incluir enlaces a commits y comparaciones

### 2. **Información Útil**
- Añadir contexto para cambios importantes
- Incluir breaking changes claramente marcados
- Mencionar dependencias actualizadas
- Destacar mejoras de seguridad

### 3. **Navegación Fácil**
- Tabla de contenidos para changelogs largos
- Enlaces de navegación rápida
- Secciones colapsables para versiones antiguas
- Búsqueda fácil por tipo de cambio

### 4. **Estadísticas y Métricas**
- Número total de cambios por versión
- Tiempo entre versiones
- Contribuidores de la versión
- Métricas específicas del proyecto

## ✅ **Checklist para Changelog de Calidad**

### Antes de Publicar
- [ ] **Formato**: Todos los elementos siguen el formato establecido
- [ ] **Enlaces**: Todos los enlaces funcionan correctamente
- [ ] **Emojis**: Emojis consistentes y apropiados
- [ ] **Agrupación**: Cambios agrupados lógicamente
- [ ] **Contexto**: Información suficiente para entender cambios
- [ ] **Breaking Changes**: Claramente marcados y explicados
- [ ] **Fechas**: Fechas correctas en formato ISO
- [ ] **Versiones**: Versionado semántico correcto

### Después de Publicar
- [ ] **Navegación**: Verificar que la navegación funciona
- [ ] **Renderizado**: Confirmar que se ve bien en GitHub
- [ ] **Enlaces**: Verificar enlaces a releases y commits
- [ ] **Notificaciones**: Confirmar que se enviaron notificaciones
- [ ] **Integración**: Verificar integración con otras herramientas

## 🚀 **Comandos Útiles**

### Generar Estadísticas
```bash
# Contar archivos por tipo
find recipes/ -name "*.html" | wc -l

# Contar commits por autor
git shortlog -sn

# Cambios desde última versión
git log $(git describe --tags --abbrev=0)..HEAD --oneline

# Estadísticas de líneas de código
git ls-files | xargs wc -l
```

### Verificar Formato
```bash
# Verificar enlaces rotos en changelog
markdown-link-check CHANGELOG.md

# Validar formato markdown
markdownlint CHANGELOG.md

# Verificar commits convencionales
conventional-changelog-cli --verify
```

---

**Resultado**: Changelog profesional, atractivo e informativo que mejora la experiencia del usuario y facilita el seguimiento de cambios del proyecto 📝✨