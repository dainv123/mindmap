# 🎨 CSS Advanced - Complete Knowledge Map

## 🧠 Mindmap Overview
```
CSS Advanced
├── 🎯 Layout Systems
│   ├── Flexbox → One-dimensional layout
│   ├── Grid → Two-dimensional layout
│   ├── CSS Columns → Multi-column text layout
│   └── Positioning → Absolute, relative, fixed
├── 🎭 Advanced Selectors
│   ├── Pseudo-classes → :hover, :focus, :nth-child
│   ├── Pseudo-elements → ::before, ::after, ::first-line
│   ├── Attribute Selectors → [attr], [attr=value]
│   └── Combinators → Descendant, child, adjacent
├── ⚡ Animations & Transitions
│   ├── Transitions → Property changes over time
│   ├── Animations → Keyframe-based animations
│   ├── Transforms → 2D/3D transformations
│   └── Performance → Hardware acceleration
├── 🎨 Modern CSS Features
│   ├── CSS Variables → Custom properties
│   ├── CSS Functions → calc(), var(), clamp()
│   ├── Logical Properties → Writing mode support
│   └── Container Queries → Component-based styling
└── 🚀 Performance & Optimization
    ├── Critical CSS → Above-the-fold styles
    ├── CSS-in-JS → Dynamic styling
    ├── CSS Modules → Scoped styles
    └── PostCSS → CSS processing tools
```

## 📋 Table of Contents
- [Layout Systems](#layout-systems)
- [Advanced Selectors](#advanced-selectors)
- [Animations & Transitions](#animations--transitions)
- [Modern CSS Features](#modern-css-features)
- [Performance & Optimization](#performance--optimization)
- [Common Interview Questions](#common-interview-questions)

## 🎯 Layout Systems

### Flexbox
Flexbox cho one-dimensional layout:

```css
/* Basic flexbox container */
.flex-container {
  display: flex;
  flex-direction: row; /* row, row-reverse, column, column-reverse */
  justify-content: space-between; /* flex-start, center, flex-end, space-around */
  align-items: center; /* stretch, flex-start, center, flex-end, baseline */
  flex-wrap: wrap; /* nowrap, wrap, wrap-reverse */
  gap: 20px; /* Space between flex items */
}

/* Flexbox items */
.flex-item {
  flex: 1; /* flex-grow: 1, flex-shrink: 1, flex-basis: 0% */
  flex-grow: 0; /* Don't grow */
  flex-shrink: 1; /* Allow shrinking */
  flex-basis: auto; /* Initial size */
  
  /* Shorthand: flex: <grow> <shrink> <basis> */
  flex: 0 1 200px; /* Don't grow, allow shrink, start at 200px */
}

/* Responsive flexbox */
.responsive-flex {
  display: flex;
  flex-direction: column;
}

@media (min-width: 768px) {
  .responsive-flex {
    flex-direction: row;
  }
}

/* Centering with flexbox */
.center-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

/* Navigation with flexbox */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

.nav-links {
  display: flex;
  gap: 2rem;
  list-style: none;
}

/* Card layout */
.card-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.card {
  flex: 1 1 300px; /* Grow, shrink, basis */
  min-width: 0; /* Allow shrinking below content size */
}
```

### CSS Grid
Grid cho two-dimensional layout:

```css
/* Basic grid container */
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 equal columns */
  grid-template-rows: 100px 200px; /* 2 rows with specific heights */
  gap: 20px; /* Grid gap */
  grid-template-areas: 
    "header header header"
    "sidebar main main"
    "footer footer footer";
}

/* Grid areas */
.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }

/* Responsive grid */
.responsive-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

/* Grid with named lines */
.named-grid {
  display: grid;
  grid-template-columns: [sidebar-start] 200px [main-start] 1fr [main-end];
  grid-template-rows: [header-start] 80px [content-start] 1fr [footer-start] 60px;
}

.sidebar {
  grid-column: sidebar-start;
  grid-row: header-start / footer-start;
}

.main {
  grid-column: main-start / main-end;
  grid-row: content-start / footer-start;
}

/* Auto-filling grid */
.auto-fill-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

/* Grid alignment */
.aligned-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  justify-items: center; /* Align items horizontally */
  align-items: center; /* Align items vertically */
  justify-content: center; /* Align grid in container */
  align-content: center; /* Align grid in container */
}
```

### CSS Columns
Multi-column text layout:

```css
/* Basic columns */
.column-text {
  column-count: 3;
  column-gap: 2rem;
  column-rule: 1px solid #ccc;
  column-width: 200px; /* Alternative to column-count */
}

/* Column breaks */
.column-break {
  break-inside: avoid; /* Prevent breaking */
  break-before: column; /* Force break before */
  break-after: column; /* Force break after */
}

/* Column spans */
.span-all {
  column-span: all; /* Span all columns */
}

/* Responsive columns */
.responsive-columns {
  column-count: 1;
  column-gap: 1rem;
}

@media (min-width: 768px) {
  .responsive-columns {
    column-count: 2;
    column-gap: 2rem;
  }
}

@media (min-width: 1200px) {
  .responsive-columns {
    column-count: 3;
    column-gap: 3rem;
  }
}
```

## 🎭 Advanced Selectors

### Pseudo-classes
Dynamic state selectors:

```css
/* Basic pseudo-classes */
.button:hover {
  background-color: #007bff;
  transform: translateY(-2px);
  transition: all 0.3s ease;
}

.button:focus {
  outline: 2px solid #007bff;
  outline-offset: 2px;
}

.button:active {
  transform: translateY(0);
}

/* Form pseudo-classes */
.input:valid {
  border-color: #28a745;
}

.input:invalid {
  border-color: #dc3545;
}

.input:required {
  border-width: 2px;
}

/* Structural pseudo-classes */
.list-item:nth-child(odd) {
  background-color: #f8f9fa;
}

.list-item:nth-child(even) {
  background-color: #ffffff;
}

.list-item:first-child {
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.list-item:last-child {
  border-bottom-left-radius: 8px;
  border-bottom-right-radius: 8px;
}

/* State pseudo-classes */
.checkbox:checked + label {
  color: #007bff;
  font-weight: bold;
}

.radio:checked + label::before {
  content: "✓ ";
  color: #28a745;
}

/* Link pseudo-classes */
.link:link {
  color: #007bff;
}

.link:visited {
  color: #6f42c1;
}

.link:hover {
  color: #0056b3;
  text-decoration: underline;
}

.link:active {
  color: #004085;
}
```

### Pseudo-elements
Generated content:

```css
/* Basic pseudo-elements */
.quote::before {
  content: """;
  font-size: 3rem;
  color: #007bff;
  float: left;
  margin-right: 0.5rem;
  line-height: 1;
}

.quote::after {
  content: """;
  font-size: 3rem;
  color: #007bff;
  float: right;
  margin-left: 0.5rem;
  line-height: 1;
}

/* First line and letter */
.article::first-line {
  font-weight: bold;
  color: #333;
}

.article::first-letter {
  font-size: 4rem;
  float: left;
  margin-right: 0.5rem;
  line-height: 1;
  color: #007bff;
}

/* Selection styling */
.text::selection {
  background-color: #007bff;
  color: white;
}

/* Placeholder styling */
.input::placeholder {
  color: #6c757d;
  font-style: italic;
}

/* Marker styling (for lists) */
.list-item::marker {
  color: #007bff;
  font-weight: bold;
}

/* Custom bullet points */
.custom-list {
  list-style: none;
}

.custom-list li::before {
  content: "▶";
  color: #007bff;
  margin-right: 0.5rem;
}

/* Tooltip with pseudo-element */
.tooltip {
  position: relative;
}

.tooltip::after {
  content: attr(data-tooltip);
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: white;
  padding: 0.5rem;
  border-radius: 4px;
  font-size: 0.875rem;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.tooltip:hover::after {
  opacity: 1;
  visibility: visible;
}
```

### Attribute Selectors
Selecting by attributes:

```css
/* Basic attribute selectors */
[type="text"] {
  border: 1px solid #ccc;
}

[type^="text"] { /* Starts with */
  border: 1px solid #007bff;
}

[type$="text"] { /* Ends with */
  border: 1px solid #28a745;
}

[type*="text"] { /* Contains */
  border: 1px solid #ffc107;
}

/* Multiple attributes */
[type="text"][required] {
  border-color: #dc3545;
}

/* Partial attribute matching */
[class~="btn"] { /* Contains word */
  display: inline-block;
  padding: 0.5rem 1rem;
}

[class|="btn"] { /* Starts with word or word- */
  background-color: #007bff;
  color: white;
}

/* Case sensitivity */
[type="text" i] { /* Case insensitive */
  border: 2px solid #007bff;
}

/* Practical examples */
/* External links */
a[href^="http"]::after {
  content: " ↗";
  color: #6c757d;
}

/* File downloads */
a[href$=".pdf"]::before {
  content: "📄 ";
}

a[href$=".zip"]::before {
  content: "📦 ";
}

/* Form validation */
input[required] {
  border-left: 4px solid #dc3545;
}

input[pattern] {
  border-left: 4px solid #ffc107;
}
```

## ⚡ Animations & Transitions

### CSS Transitions
Smooth property changes:

```css
/* Basic transitions */
.button {
  background-color: #007bff;
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: #0056b3;
}

/* Multiple properties */
.card {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: 
    transform 0.3s ease,
    box-shadow 0.3s ease,
    background-color 0.3s ease;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
  background-color: #f8f9fa;
}

/* Transition timing functions */
.ease-in {
  transition-timing-function: ease-in;
}

.ease-out {
  transition-timing-function: ease-out;
}

.ease-in-out {
  transition-timing-function: ease-in-out;
}

.cubic-bezier {
  transition-timing-function: cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

/* Delayed transitions */
.delayed {
  transition-delay: 0.2s;
}

.staggered > * {
  transition-delay: calc(var(--index) * 0.1s);
}

/* Conditional transitions */
.conditional {
  transition: opacity 0.3s ease;
}

.conditional:hover {
  opacity: 0.8;
}

.conditional:focus {
  opacity: 1;
  transition: opacity 0.1s ease; /* Faster transition */
}
```

### CSS Animations
Keyframe-based animations:

```css
/* Basic keyframe animation */
@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.slide-in {
  animation: slideIn 0.5s ease-out;
}

/* Complex keyframe animation */
@keyframes bounce {
  0%, 20%, 53%, 80%, 100% {
    transform: translateY(0);
  }
  40%, 43% {
    transform: translateY(-30px);
  }
  70% {
    transform: translateY(-15px);
  }
  90% {
    transform: translateY(-4px);
  }
}

.bounce {
  animation: bounce 1s ease-in-out;
}

/* Animation properties */
.animated {
  animation-name: slideIn;
  animation-duration: 0.5s;
  animation-timing-function: ease-out;
  animation-delay: 0.2s;
  animation-iteration-count: 3;
  animation-direction: alternate;
  animation-fill-mode: both;
  animation-play-state: running;
}

/* Shorthand */
.animated-shorthand {
  animation: slideIn 0.5s ease-out 0.2s 3 alternate both;
}

/* Pause/play animation */
.animated:hover {
  animation-play-state: paused;
}

/* Infinite animation */
.spinner {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

/* Staggered animations */
.staggered > * {
  animation: fadeIn 0.5s ease-out forwards;
  opacity: 0;
}

.staggered > *:nth-child(1) { animation-delay: 0.1s; }
.staggered > *:nth-child(2) { animation-delay: 0.2s; }
.staggered > *:nth-child(3) { animation-delay: 0.3s; }
```

### CSS Transforms
2D and 3D transformations:

```css
/* 2D Transforms */
.scale {
  transform: scale(1.2);
}

.scale-x {
  transform: scaleX(1.5);
}

.scale-y {
  transform: scaleY(0.8);
}

.rotate {
  transform: rotate(45deg);
}

.translate {
  transform: translate(20px, 10px);
}

.translate-x {
  transform: translateX(20px);
}

.translate-y {
  transform: translateY(-10px);
}

.skew {
  transform: skew(10deg, 5deg);
}

/* Multiple transforms */
.multiple {
  transform: translate(20px, 10px) rotate(45deg) scale(1.2);
}

/* 3D Transforms */
.perspective {
  perspective: 1000px;
}

.rotate-3d {
  transform: rotateX(45deg) rotateY(45deg);
}

.translate-3d {
  transform: translate3d(20px, 10px, 50px);
}

.scale-3d {
  transform: scale3d(1.2, 0.8, 1.5);
}

/* Transform origin */
.origin-center {
  transform-origin: center;
}

.origin-top-left {
  transform-origin: top left;
}

.origin-custom {
  transform-origin: 25% 75%;
}

/* Hardware acceleration */
.hardware-accelerated {
  transform: translateZ(0);
  will-change: transform;
}

/* Transform with transitions */
.smooth-transform {
  transition: transform 0.3s ease;
}

.smooth-transform:hover {
  transform: scale(1.1) rotate(5deg);
}
```

## 🎨 Modern CSS Features

### CSS Variables (Custom Properties)
Dynamic CSS values:

```css
/* Root variables */
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  --danger-color: #dc3545;
  --warning-color: #ffc107;
  --info-color: #17a2b8;
  
  --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  --font-size-base: 1rem;
  --line-height-base: 1.5;
  
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 3rem;
  
  --border-radius: 0.375rem;
  --box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
  --transition: all 0.15s ease-in-out;
}

/* Using variables */
.button {
  background-color: var(--primary-color);
  color: white;
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: var(--border-radius);
  font-family: var(--font-family);
  transition: var(--transition);
}

.button:hover {
  background-color: color-mix(in srgb, var(--primary-color) 80%, black);
  transform: translateY(-2px);
  box-shadow: var(--box-shadow);
}

/* Fallback values */
.text {
  color: var(--text-color, #333);
  font-size: var(--font-size, 1rem);
}

/* Dynamic variables */
.theme-dark {
  --bg-color: #1a1a1a;
  --text-color: #ffffff;
  --border-color: #333333;
}

.theme-light {
  --bg-color: #ffffff;
  --text-color: #333333;
  --border-color: #e0e0e0;
}

/* Component-specific variables */
.card {
  --card-padding: var(--spacing-md);
  --card-border-radius: var(--border-radius);
  --card-shadow: var(--box-shadow);
  
  padding: var(--card-padding);
  border-radius: var(--card-border-radius);
  box-shadow: var(--card-shadow);
}

/* CSS variables in JavaScript */
:root {
  --scrollbar-width: 0px;
}

/* JavaScript can update this */
document.documentElement.style.setProperty('--scrollbar-width', '16px');
```

### CSS Functions
Built-in CSS functions:

```css
/* calc() function */
.responsive-width {
  width: calc(100% - 2rem);
  height: calc(100vh - 100px);
  margin: calc(var(--spacing-md) / 2);
}

/* min() and max() functions */
.responsive-text {
  font-size: min(4vw, 2rem);
  width: max(300px, 50%);
}

/* clamp() function */
.fluid-text {
  font-size: clamp(1rem, 4vw, 2rem);
  line-height: clamp(1.2, 1.5, 2);
}

/* var() function with fallbacks */
.text {
  color: var(--text-color, var(--fallback-color, #333));
}

/* attr() function */
.tooltip::after {
  content: attr(data-tooltip);
}

/* url() function */
.background {
  background-image: url('data:image/svg+xml,<svg>...</svg>');
}

/* color-mix() function */
.button-secondary {
  background-color: color-mix(in srgb, var(--primary-color) 20%, white);
}

.button-dark {
  background-color: color-mix(in srgb, var(--primary-color) 80%, black);
}

/* Custom property with calc */
:root {
  --header-height: 60px;
  --sidebar-width: 250px;
  --main-content-height: calc(100vh - var(--header-height));
  --main-content-width: calc(100vw - var(--sidebar-width));
}
```

### Logical Properties
Writing mode support:

```css
/* Logical properties for RTL support */
.text {
  margin-inline-start: 1rem; /* margin-left in LTR, margin-right in RTL */
  margin-inline-end: 1rem;   /* margin-right in LTR, margin-left in RTL */
  padding-block-start: 1rem; /* padding-top */
  padding-block-end: 1rem;   /* padding-bottom */
}

/* Logical sizing */
.container {
  width: fit-content;
  height: fit-content;
  min-width: min-content;
  max-width: max-content;
}

/* Logical borders */
.border {
  border-inline-start: 2px solid var(--primary-color);
  border-block-end: 1px solid var(--border-color);
}

/* Logical positioning */
.positioned {
  inset-block-start: 0;    /* top in horizontal writing mode */
  inset-inline-end: 0;     /* right in LTR, left in RTL */
}

/* Writing mode support */
.vertical-text {
  writing-mode: vertical-rl;
  text-orientation: mixed;
}

/* Logical flow */
.flow {
  display: flex;
  flex-direction: column;
}

[dir="rtl"] .flow {
  flex-direction: column-reverse;
}
```

## 🚀 Performance & Optimization

### Critical CSS
Above-the-fold styles:

```css
/* Critical CSS for above-the-fold content */
.critical {
  /* Inline critical styles */
  display: block;
  width: 100%;
  height: 100vh;
  background-color: #ffffff;
}

/* Non-critical styles loaded asynchronously */
.non-critical {
  /* These styles can be loaded later */
  animation: fadeIn 0.5s ease-out;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

/* CSS loading strategies */
/* 1. Inline critical CSS */
/* 2. Async load non-critical CSS */
/* 3. Preload important CSS files */

/* Preload critical resources */
<link rel="preload" href="critical.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="critical.css"></noscript>
```

### CSS-in-JS
Dynamic styling in JavaScript:

```css
/* CSS-in-JS example with styled-components approach */
/* This would be in JavaScript, but showing the CSS output */

.dynamic-button {
  background-color: var(--button-bg, #007bff);
  color: var(--button-color, white);
  padding: var(--button-padding, 0.5rem 1rem);
  border-radius: var(--button-radius, 0.375rem);
  transition: all 0.3s ease;
}

.dynamic-button:hover {
  background-color: var(--button-hover-bg, #0056b3);
  transform: translateY(-2px);
}

/* CSS custom properties for dynamic values */
:root {
  --theme-color: #007bff;
  --theme-hover: #0056b3;
}

/* JavaScript can update these */
document.documentElement.style.setProperty('--theme-color', newColor);
```

### CSS Modules
Scoped styles:

```css
/* CSS Modules automatically scope classes */
.button {
  background-color: #007bff;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
}

/* Generated class name: Button_button_abc123 */
/* This prevents style conflicts */

/* Composition with CSS Modules */
.common {
  /* Common styles */
}

.primary {
  composes: common;
  background-color: #007bff;
}

.secondary {
  composes: common;
  background-color: #6c757d;
}
```

### PostCSS
CSS processing tools:

```css
/* PostCSS can transform modern CSS to browser-compatible CSS */

/* Input (modern CSS) */
.custom-property {
  color: var(--primary-color);
}

/* Output (processed CSS) */
.custom-property {
  color: #007bff;
}

/* Autoprefixer example */
.flexbox {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* PostCSS adds vendor prefixes */
.flexbox {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
}
```

## ❓ Common Interview Questions

### 1. Flexbox vs Grid
**Q: Khi nào sử dụng Flexbox vs CSS Grid?**

**A:** 
- **Flexbox**: One-dimensional layout (row or column), navigation, form elements
- **Grid**: Two-dimensional layout, page layouts, complex designs
- **Flexbox**: Better for component-level layout
- **Grid**: Better for page-level layout
- **Use both**: Grid for overall layout, Flexbox for components

### 2. CSS Performance
**Q: Cách optimize CSS performance?**

**A:** 
- **Critical CSS**: Inline above-the-fold styles
- **Minification**: Remove whitespace and comments
- **Concatenation**: Combine multiple CSS files
- **Gzip compression**: Reduce file size
- **Avoid expensive properties**: transform, opacity instead of position changes
- **Use will-change**: Hint to browser about animations

### 3. CSS Architecture
**Q: CSS architecture best practices?**

**A:** 
- **BEM methodology**: Block__Element--Modifier
- **CSS Modules**: Scoped styles
- **CSS-in-JS**: Dynamic styling
- **Utility-first**: Tailwind CSS approach
- **Component-based**: Styles with components
- **Consistent naming**: Follow conventions

### 4. Responsive Design
**Q: CSS techniques for responsive design?**

**A:** 
- **Media queries**: Breakpoint-based styles
- **Flexbox/Grid**: Flexible layouts
- **CSS custom properties**: Dynamic values
- **Container queries**: Component-based breakpoints
- **Fluid typography**: clamp(), min(), max()
- **Mobile-first**: Start with mobile, enhance for larger screens

### 5. CSS Animations
**Q: CSS animations vs JavaScript animations?**

**A:** 
- **CSS animations**: Better performance, hardware acceleration
- **JavaScript animations**: More control, complex logic
- **Use CSS**: Simple animations, hover effects
- **Use JavaScript**: Complex animations, user interactions
- **Performance**: CSS uses GPU, JavaScript uses CPU
- **Browser support**: CSS has better support

## 📚 Resources
- [CSS Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
- [CSS Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout)
- [CSS Animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations)
- [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
- [CSS Container Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries) 