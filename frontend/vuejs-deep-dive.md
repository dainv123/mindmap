# ⚡ Vue.js Deep Dive

## 🏗️ Vue 3 Composition API
- **Setup Function**: Component logic với `<script setup>`
- **Reactive References**: `ref()`, `reactive()` cho reactive state
- **Computed Properties**: `computed()` cho derived state
- **Lifecycle Hooks**: `onMounted()`, `onUnmounted()`

## 🔄 State Management
- **Pinia**: Modern state management cho Vue 3
- **Vuex**: Legacy state management cho Vue 2
- **Provide/Inject**: Component communication pattern
- **Composables**: Reusable logic functions

## 🎯 Advanced Patterns
- **Render Functions**: Programmatic rendering với `h()`
- **Custom Directives**: `v-custom` directives
- **Teleport**: Move DOM elements với `<Teleport>`
- **Suspense**: Async components handling

## 🚀 Performance & Optimization
- **Virtual DOM**: Efficient updates với new Virtual DOM
- **Tree Shaking**: Bundle optimization với ES modules
- **Lazy Loading**: Code splitting với `defineAsyncComponent`
- **Memoization**: `v-memo` cho prevent unnecessary re-renders

## 🧪 Testing & Debugging
- **Vue Test Utils**: Component testing
- **Vitest**: Unit testing
- **Vue DevTools**: Development tools
- **Error Handling**: Global error handling

## ❓ Common Interview Questions

### 1. Composition API vs Options API
**Q: Khi nào sử dụng Composition API vs Options API?**
**A:** Composition API cho complex logic, TypeScript, composables. Options API cho simple components, Vue 2 migration.

### 2. Vue 3 vs Vue 2
**Q: Những thay đổi chính trong Vue 3?**
**A:** Performance improvements, Composition API, Teleport, Suspense, Fragments, better TypeScript support.

### 3. State Management
**Q: Pinia vs Vuex - khi nào sử dụng?**
**A:** Pinia cho Vue 3, TypeScript, better devtools. Vuex cho Vue 2, complex patterns, legacy codebases.

### 4. Performance Optimization
**Q: Cách optimize Vue.js performance?**
**A:** Use v-memo, lazy loading, shallowRef, avoid unnecessary re-renders, code splitting, tree shaking.

### 5. Testing Vue Components
**Q: Cách test Vue components hiệu quả?**
**A:** Unit test methods, integration test interactions, use Vue Test Utils, mock dependencies, test edge cases. 