# 🎨 Paleta de Colores - Odin Recipes

## Colores Implementados

Esta paleta de colores está inspirada en tonos cálidos y terrosos que evocan la atmósfera de la cocina tradicional española.

### 🧡 Color Principal - Naranja Terracota
Usado para encabezados principales y elementos destacados.

```css
--primary-lighter: #F4A261  /* Tono más claro */
--primary-light:   #E8845F  /* Tono claro */
--primary:         #D9653B  /* Color base */
--primary-dark:    #C44E2A  /* Tono oscuro */
--primary-darker:  #A63F22  /* Tono más oscuro */
```

**Uso:**
- Títulos H2 y elementos de énfasis
- Bordes de listas ordenadas
- Encabezados de sección

### 💛 Color Secundario - Amarillo Dorado
Representa el azafrán y los tonos dorados de la cocina española.

```css
--secondary-lighter: #FFE4A3  /* Tono más claro */
--secondary-light:   #FFD87F  /* Tono claro */
--secondary:         #F4C430  /* Color base */
--secondary-dark:    #D9A91A  /* Tono oscuro */
--secondary-darker:  #B8900F  /* Tono más oscuro */
```

**Uso:**
- Bordes inferiores de encabezados
- Bordes de imágenes
- Fondos de sección de consejos
- Acentos decorativos

### 💚 Color Acento - Verde Oliva
Inspirado en el aceite de oliva y hierbas mediterráneas.

```css
--accent-lighter: #B8C89F  /* Tono más claro */
--accent-light:   #9FB584  /* Tono claro */
--accent:         #7A9B57  /* Color base */
--accent-dark:    #5F7A41  /* Tono oscuro */
--accent-darker:  #475A30  /* Tono más oscuro */
```

**Uso:**
- Enlaces de navegación
- Bordes de listas de ingredientes
- Botones secundarios
- Títulos H3

### 🤎 Color de Fondo - Crema
Tonos suaves y cálidos que crean una base acogedora.

```css
--bg-lightest: #FFFFFF  /* Blanco puro */
--bg-lighter:  #FDF8F3  /* Crema muy claro */
--bg-light:    #F5EBE0  /* Crema claro */
--bg:          #EDDFCC  /* Color base */
--bg-dark:     #E5D4BB  /* Crema oscuro */
--bg-darker:   #D9C4A8  /* Crema más oscuro */
```

**Uso:**
- Fondo general del sitio
- Tarjetas de recetas
- Elementos de lista
- Secciones de contenido

### 🖤 Color de Sombra - Marrón Oscuro
Aporta profundidad y contraste profesional.

```css
--shadow-light:  #8B6F5E  /* Sombra suave */
--shadow:        #5E3A2A  /* Color base */
--shadow-dark:   #4A2E21  /* Sombra oscura */
--shadow-darker: #361F17  /* Sombra profunda */
```

**Uso:**
- Sombras de elementos (box-shadow)
- Efectos de profundidad
- Sombras de texto

### 📝 Color de Texto
```css
--text-light: #5E3A2A  /* Texto secundario */
--text:       #2E1A12  /* Texto principal */
--text-dark:  #1A0F09  /* Texto muy oscuro */
```

---

## 📋 Ejemplo de Aplicación en HTML

### Cabecera de Receta

```html
<header>
    <h1>Paella Valenciana</h1>
    <p>Receta tradicional española auténtica</p>
</header>
```

**Resultado visual:**
- Fondo degradado de naranja terracota (primary → primary-dark)
- Texto blanco con sombra oscura
- Padding y border-radius para suavidad
- Box-shadow profunda para efecto de elevación

---

### Sección de Ingredientes

```html
<section>
    <h2>Ingredientes</h2>
    <ul>
        <li>400g de arroz bomba o Calasparra</li>
        <li>1 pollo troceado (1.2 kg aprox.)</li>
        <li>200g de judías verdes</li>
        <!-- ... más ingredientes ... -->
    </ul>
</section>
```

**Resultado visual:**
- H2 en color naranja terracota (primary)
- Borde inferior amarillo dorado (secondary)
- Items de lista con:
  - Fondo crema (bg)
  - Borde izquierdo verde oliva (accent)
  - Icono de emoji 🍽️
  - Border-radius suave

---

### Lista de Pasos (Ordenada)

```html
<section>
    <h2>Pasos</h2>
    <ol>
        <li>
            <strong>Preparar la paellera:</strong> 
            Calienta el aceite de oliva en una paellera...
        </li>
        <li>
            <strong>Dorar la carne:</strong> 
            Sala el pollo y el conejo...
        </li>
        <!-- ... más pasos ... -->
    </ol>
</section>
```

**Resultado visual:**
- Contador circular naranja terracota con número blanco
- Fondo crema (bg)
- Borde izquierdo naranja (primary)
- Texto en negrita oscuro (primary-dark)
- Espaciado generoso entre pasos

---

### Sección de Consejos

```html
<section class="recipe-section">
    <h2>Consejos</h2>
    <div class="tips">
        <ul>
            <li>Nunca remuevas el arroz una vez añadido</li>
            <li>La proporción ideal es 1:2.5 de arroz:caldo</li>
            <!-- ... más consejos ... -->
        </ul>
    </div>
</section>
```

**Resultado visual:**
- Fondo de sección crema claro (bg-lighter)
- Cuadro de consejos amarillo claro (secondary-lighter)
- Borde izquierdo amarillo oscuro (secondary-dark)
- Icono de bombilla 💡
- Padding generoso

---

### Imagen de Receta

```html
<figure>
    <img src="../images/paella.jpg" 
         alt="Paella Valenciana tradicional" 
         width="400">
</figure>
```

**Resultado visual:**
- Border-radius redondeado
- Borde amarillo dorado (secondary-light)
- Box-shadow media para profundidad
- Centrado automático
- Responsive (max-width: 100%)

---

### Enlaces de Navegación

```html
<nav>
    <a href="../index.html" class="nav-link">← Volver a Recetas</a>
</nav>
```

**Resultado visual:**
- Fondo verde oliva (accent)
- Texto blanco
- Padding interno
- Border-radius
- Efecto hover:
  - Cambio a verde oliva oscuro (accent-dark)
  - Escala ligeramente (transform: scale(1.05))
  - Sin subrayado

---

## 🎯 Variables de Utilidad

### Espaciado
```css
--spacing-xs: 0.5rem   /* 8px */
--spacing-sm: 1rem     /* 16px */
--spacing-md: 1.5rem   /* 24px */
--spacing-lg: 2rem     /* 32px */
--spacing-xl: 3rem     /* 48px */
```

### Bordes Redondeados
```css
--border-radius-sm: 4px
--border-radius-md: 8px
--border-radius-lg: 12px
```

### Sombras
```css
--shadow-sm: 0 2px 4px rgba(94, 58, 42, 0.1)
--shadow-md: 0 4px 8px rgba(94, 58, 42, 0.15)
--shadow-lg: 0 8px 16px rgba(94, 58, 42, 0.2)
```

---

## 📱 Responsive Design

El diseño es completamente responsive y se adapta a dispositivos móviles:

```css
@media (max-width: 768px) {
  /* Reduce tamaños de fuente */
  h1 { font-size: 2rem; }
  h2 { font-size: 1.5rem; }
  
  /* Reduce padding en contenedores */
  .container { padding: var(--spacing-md); }
  header { padding: var(--spacing-md); }
}
```

---

## ✨ Características de la Paleta

1. **Cálida y acogedora**: Tonos terrosos que evocan la cocina tradicional
2. **Alto contraste**: Texto oscuro sobre fondos claros para legibilidad
3. **Profesional**: Sombras sutiles y gradientes suaves
4. **Accesible**: Contraste adecuado para WCAG
5. **Flexible**: Múltiples tonos para diferentes usos
6. **Cohesiva**: Colores armoniosos que funcionan juntos

---

## 🔧 Cómo Usar las Variables

En cualquier parte de tu CSS, puedes usar:

```css
.mi-elemento {
  background-color: var(--primary);
  color: var(--bg-lightest);
  padding: var(--spacing-md);
  border-radius: var(--border-radius-md);
  box-shadow: var(--shadow-md);
}
```

---

## 📚 Ejemplo Completo de Tarjeta

```html
<div class="recipe-card">
    <h3>Paella Valenciana</h3>
    <p class="description">
        La paella valenciana es uno de los platos más emblemáticos...
    </p>
    <a href="recipes/paella.html" class="nav-link">Ver Receta</a>
</div>
```

**CSS aplicado automáticamente:**
- Fondo crema (bg)
- Borde amarillo claro (secondary-light)
- Sombra media
- Efecto hover con elevación
- Padding y border-radius
- Transiciones suaves

---

¡Disfruta de la nueva paleta de colores cálida y profesional! 🥘✨
