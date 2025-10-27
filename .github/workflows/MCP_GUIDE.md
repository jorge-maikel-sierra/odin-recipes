# 🔌 Guía de Servidores MCP para Odin Recipes

## 🎯 **Servidores MCP Configurados**

### **📁 Sistema de Archivos (Filesystem)**
```json
"filesystem": {
  "command": "npx",
  "args": ["@modelcontextprotocol/server-filesystem", "--base-path", "./"],
  "env": {
    "ALLOWED_DIRECTORIES": "recipes,images,.github,docs"
  }
}
```

**🔧 Qué puedes hacer:**
- ✅ Leer y analizar archivos HTML de recetas
- ✅ Crear nuevas recetas siguiendo patrones existentes
- ✅ Modificar estructura de navegación
- ✅ Analizar configuración de GitHub Actions

**💬 Comandos útiles:**
```bash
@filesystem "Analizar la estructura de todas las recetas HTML"
@filesystem "Crear nueva receta de fabada asturiana siguiendo el patrón de paella.html"
@filesystem "Optimizar todos los archivos HTML para mejor SEO"
@filesystem "Revisar consistencia en formato de todas las recetas"
```

### **🌐 Web Fetch (Investigación)**
```json
"web-fetch": {
  "command": "npx", 
  "args": ["@modelcontextprotocol/server-fetch"]
}
```

**🔧 Qué puedes hacer:**
- ✅ Investigar recetas tradicionales auténticas
- ✅ Verificar ingredientes y técnicas culinarias
- ✅ Buscar información histórica de platos
- ✅ Analizar sitios web de cocina española

**💬 Comandos útiles:**
```bash
@web-fetch "Investigar la auténtica receta de cocido madrileño"
@web-fetch "Buscar variantes regionales del gazpacho en diferentes provincias"
@web-fetch "Verificar ingredientes tradicionales de la paella valenciana"
@web-fetch "Investigar técnicas de elaboración del jamón ibérico"
```

### **🧠 Pensamiento Secuencial (Sequential Thinking)**
```json
"sequential-thinking": {
  "command": "npx",
  "args": ["@modelcontextprotocol/server-sequential-thinking"]
}
```

**🔧 Qué puedes hacer:**
- ✅ Planificar estructura completa de nuevas recetas
- ✅ Resolver problemas complejos de organización
- ✅ Optimizar flujo de trabajo de desarrollo
- ✅ Diseñar estrategias de contenido

**💬 Comandos útiles:**
```bash
@sequential-thinking "Planificar estructura para añadir 20 recetas organizadas por regiones"
@sequential-thinking "Optimizar SEO del sitio para aparecer en búsquedas de recetas españolas"
@sequential-thinking "Diseñar sistema de navegación por ingredientes principales"
```

### **🤖 Puppeteer (Automatización Web)**
```json
"puppeteer": {
  "command": "npx",
  "args": ["puppeteer-mcp-server"]
}
```

**🔧 Qué puedes hacer:**
- ✅ Hacer capturas de pantalla de las recetas
- ✅ Probar navegación del sitio automáticamente
- ✅ Verificar responsive design
- ✅ Extraer datos de sitios de referencia

**💬 Comandos útiles:**
```bash
@puppeteer "Hacer captura de pantalla de todas las páginas de recetas"
@puppeteer "Probar navegación completa del sitio y detectar enlaces rotos"
@puppeteer "Verificar que el sitio se ve bien en móvil"
```

## 🚀 **Casos de Uso Específicos**

### **1. Crear Nueva Receta Completa**
```bash
# Paso 1: Investigar
@web-fetch "Investigar receta tradicional de marmitako vasco"

# Paso 2: Planificar
@sequential-thinking "Planificar estructura HTML para receta de marmitako siguiendo patrones existentes"

# Paso 3: Crear archivo
@filesystem "Crear recipes/marmitako-vasco.html con la información investigada"

# Paso 4: Actualizar navegación
@filesystem "Añadir enlace a marmitako en index.html"
```

### **2. Optimización SEO Masiva**
```bash
# Análisis
@filesystem "Analizar meta tags de todas las recetas HTML"

# Investigación de keywords
@web-fetch "Investigar palabras clave populares para recetas españolas"

# Planificación
@sequential-thinking "Crear estrategia SEO para mejorar ranking en Google"

# Implementación
@filesystem "Optimizar meta tags de todas las recetas con nuevas keywords"
```

### **3. Control de Calidad**
```bash
# Verificar estructura
@filesystem "Verificar que todas las recetas siguen el mismo formato HTML"

# Probar funcionamiento
@puppeteer "Probar navegación completa y accesibilidad del sitio"

# Análisis de contenido
@sequential-thinking "Identificar inconsistencias en el contenido de las recetas"
```

### **4. Investigación Cultural**
```bash
# Investigar autenticidad
@web-fetch "Verificar autenticidad de ingredientes de tortilla española"

# Contexto histórico
@web-fetch "Buscar historia y origen de cada receta del sitio"

# Planificar mejoras
@sequential-thinking "Añadir secciones de historia cultural a cada receta"
```

## 📋 **Configuración en VS Code**

### **settings.json**
```json
{
  "github.copilot.enable": {
    "*": true
  },
  "github.copilot.advanced": {
    "mcp.enabled": true,
    "mcp.configPath": ".mcp-settings.json"
  }
}
```

## 🎯 **Flujo de Trabajo Recomendado**

### **Para Nuevas Recetas:**
1. **🔍 Investigar** → `@web-fetch "Buscar receta auténtica de [plato]"`
2. **🧠 Planificar** → `@sequential-thinking "Diseñar estructura para [receta]"`
3. **📁 Crear** → `@filesystem "Crear archivo HTML siguiendo patrón"`
4. **🔗 Integrar** → `@filesystem "Actualizar navegación y enlaces"`
5. **✅ Verificar** → `@puppeteer "Probar nueva receta en el sitio"`

### **Para Mantenimiento:**
1. **📊 Analizar** → `@filesystem "Revisar consistencia de todas las recetas"`
2. **🌐 Investigar** → `@web-fetch "Verificar información actualizada"`
3. **🎯 Optimizar** → `@sequential-thinking "Mejorar SEO y estructura"`
4. **🧪 Probar** → `@puppeteer "Verificar funcionamiento completo"`

## ⚡ **Comandos Rápidos**

### **Análisis General:**
```bash
@filesystem "¿Qué archivos HTML necesitan optimización?"
@sequential-thinking "¿Cómo puedo mejorar la estructura del sitio?"
@puppeteer "¿Hay problemas de navegación en el sitio?"
```

### **Investigación:**
```bash
@web-fetch "Buscar 5 recetas tradicionales españolas para añadir"
@web-fetch "¿Cuáles son las técnicas culinarias más auténticas de [región]?"
```

### **Creación de Contenido:**
```bash
@filesystem "Crear nueva receta de [plato] en HTML"
@filesystem "Optimizar SEO de todas las recetas existentes"
@filesystem "Crear página de índice por regiones"
```

## 🔒 **Seguridad y Permisos**

- ✅ **Filesystem**: Solo acceso a carpetas permitidas (`recipes`, `images`, `.github`, `docs`)
- ✅ **Web-fetch**: Solo lectura de contenido web público
- ✅ **Puppeteer**: Sandbox local para pruebas
- ✅ **Sequential-thinking**: Sin acceso a datos externos

## 📈 **Beneficios para Tu Proyecto**

1. **🔄 Automatización**: Creación de recetas siguiendo patrones consistentes
2. **📚 Investigación**: Verificación de autenticidad cultural y gastronómica
3. **🎯 SEO**: Optimización inteligente para mejor visibilidad
4. **✅ Calidad**: Control automático de consistencia y funcionamiento
5. **⚡ Velocidad**: Desarrollo más rápido con asistencia inteligente

---

**¡Con estos servidores MCP tu proyecto de recetas españolas será mucho más potente y automatizado!** 🚀🍽️