# 💚 Vue.js Deep Dive - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Vue.js Deep Dive
├── 🎯 Core Concepts
│   ├── Reactive System → Data reactivity, watchers
│   ├── Virtual DOM → Efficient DOM updates
│   ├── Component System → Reusable components
│   └── Lifecycle Hooks → Component lifecycle management
├── 🏗️ Composition API
│   ├── Setup Function → Component logic organization
│   ├── Reactive References → ref(), reactive()
│   ├── Computed Properties → Computed, watchEffect
│   └── Lifecycle Hooks → onMounted, onUnmounted
├── 🔄 State Management
│   ├── Pinia → Modern state management
│   ├── Vuex → Centralized state management
│   ├── Provide/Inject → Dependency injection
│   └── Event Bus → Component communication
├── ⚡ Performance
│   ├── Lazy Loading → Code splitting, dynamic imports
│   ├── Virtual Scrolling → Large list optimization
│   ├── Memoization → Computed caching
│   └── Tree Shaking → Unused code elimination
└── 🛠️ Ecosystem & Tools
    ├── Vue CLI → Project scaffolding
    ├── Vite → Fast build tool
    ├── Nuxt.js → Full-stack framework
    └── Testing → Vue Test Utils, Vitest
```

## 📋 Table of Contents
- [Core Concepts](#core-concepts)
- [Composition API](#composition-api)
- [State Management](#state-management)
- [Performance](#performance)
- [Ecosystem & Tools](#ecosystem--tools)
- [Common Interview Questions](#common-interview-questions)

## 🎯 Core Concepts

### Reactive System
Vue.js sử dụng reactive system để tự động cập nhật DOM khi data thay đổi:

```javascript
// Options API
export default {
  data() {
    return {
      message: 'Hello Vue!',
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  }
}

// Composition API
import { ref, reactive, computed, watch } from 'vue'

export default {
  setup() {
    const message = ref('Hello Vue!')
    const count = ref(0)
    
    const doubleCount = computed(() => count.value * 2)
    
    watch(count, (newValue, oldValue) => {
      console.log(`Count changed from ${oldValue} to ${newValue}`)
    })
    
    const increment = () => {
      count.value++
    }
    
    return {
      message,
      count,
      doubleCount,
      increment
    }
  }
}
```

### Component System
Components là building blocks của Vue.js applications:

```javascript
// Child Component
<template>
  <div class="child-component">
    <h3>{{ title }}</h3>
    <slot name="content">
      Default content
    </slot>
    <button @click="$emit('button-click', 'Hello from child')">
      Click me
    </button>
  </div>
</template>

<script>
export default {
  name: 'ChildComponent',
  props: {
    title: {
      type: String,
      required: true
    }
  },
  emits: ['button-click']
}
</script>

// Parent Component
<template>
  <div class="parent-component">
    <ChildComponent 
      title="Child Title"
      @button-click="handleChildClick"
    >
      <template #content>
        Custom content from parent
      </template>
    </ChildComponent>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue'

export default {
  components: {
    ChildComponent
  },
  methods: {
    handleChildClick(message) {
      console.log(message)
    }
  }
}
</script>
```

## 🏗️ Composition API

### Setup Function
Setup function là entry point của Composition API:

```javascript
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue'

export default {
  setup(props, context) {
    // Props destructuring
    const { title, items } = toRefs(props)
    
    // Reactive state
    const count = ref(0)
    const user = reactive({
      name: 'John',
      age: 30
    })
    
    // Computed properties
    const doubleCount = computed(() => count.value * 2)
    const userInfo = computed(() => `${user.name} (${user.age})`)
    
    // Methods
    const increment = () => {
      count.value++
    }
    
    const updateUser = (newName, newAge) => {
      user.name = newName
      user.age = newAge
    }
    
    // Lifecycle hooks
    onMounted(() => {
      console.log('Component mounted')
      // API calls, event listeners
    })
    
    onUnmounted(() => {
      console.log('Component unmounted')
      // Cleanup
    })
    
    // Expose to template
    return {
      count,
      user,
      doubleCount,
      userInfo,
      increment,
      updateUser
    }
  }
}
```

### Composables
Composables là functions có thể tái sử dụng logic:

```javascript
// useCounter.js
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)
  
  const doubleCount = computed(() => count.value * 2)
  
  const increment = () => count.value++
  const decrement = () => count.value--
  const reset = () => count.value = initialValue
  
  return {
    count,
    doubleCount,
    increment,
    decrement,
    reset
  }
}

// useApi.js
import { ref, onMounted } from 'vue'

export function useApi(url) {
  const data = ref(null)
  const loading = ref(false)
  const error = ref(null)
  
  const fetchData = async () => {
    loading.value = true
    error.value = null
    
    try {
      const response = await fetch(url)
      data.value = await response.json()
    } catch (err) {
      error.value = err.message
    } finally {
      loading.value = false
    }
  }
  
  onMounted(fetchData)
  
  return {
    data,
    loading,
    error,
    refetch: fetchData
  }
}

// Usage in component
export default {
  setup() {
    const { count, increment } = useCounter(10)
    const { data, loading, error } = useApi('/api/users')
    
    return {
      count,
      increment,
      data,
      loading,
      error
    }
  }
}
```

## 🔄 State Management

### Pinia (Modern State Management)
Pinia là state management library mới của Vue 3:

```javascript
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0,
    name: 'Counter Store'
  }),
  
  getters: {
    doubleCount: (state) => state.count * 2,
    doubleCountPlusOne: (state) => state.count * 2 + 1
  },
  
  actions: {
    increment() {
      this.count++
    },
    decrement() {
      this.count--
    },
    async incrementAsync() {
      await new Promise(resolve => setTimeout(resolve, 1000))
      this.count++
    }
  }
})

// stores/user.js
export const useUserStore = defineStore('user', () => {
  const user = ref(null)
  const isAuthenticated = computed(() => !!user.value)
  
  const login = async (credentials) => {
    try {
      const response = await api.login(credentials)
      user.value = response.user
      return { success: true }
    } catch (error) {
      return { success: false, error: error.message }
    }
  }
  
  const logout = () => {
    user.value = null
  }
  
  return {
    user,
    isAuthenticated,
    login,
    logout
  }
})

// Usage in component
export default {
  setup() {
    const counterStore = useCounterStore()
    const userStore = useUserStore()
    
    return {
      counterStore,
      userStore
    }
  }
}
```

### Provide/Inject
Provide/Inject cho dependency injection:

```javascript
// Parent component
import { provide, ref } from 'vue'

export default {
  setup() {
    const theme = ref('dark')
    const toggleTheme = () => {
      theme.value = theme.value === 'dark' ? 'light' : 'dark'
    }
    
    provide('theme', theme)
    provide('toggleTheme', toggleTheme)
    
    return {
      theme,
      toggleTheme
    }
  }
}

// Child component
import { inject } from 'vue'

export default {
  setup() {
    const theme = inject('theme', 'light') // Default value
    const toggleTheme = inject('toggleTheme')
    
    return {
      theme,
      toggleTheme
    }
  }
}
```

## ⚡ Performance

### Lazy Loading
Lazy loading để tối ưu bundle size:

```javascript
// Route-based code splitting
import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('../views/Home.vue')
  },
  {
    path: '/about',
    component: () => import('../views/About.vue')
  },
  {
    path: '/users',
    component: () => import('../views/Users.vue')
  }
]

// Component lazy loading
export default {
  components: {
    HeavyComponent: () => import('./HeavyComponent.vue')
  }
}

// Dynamic imports
const loadModule = async () => {
  const module = await import('./HeavyModule.js')
  return module.default
}
```

### Virtual Scrolling
Virtual scrolling cho large lists:

```javascript
// VirtualList.vue
<template>
  <div class="virtual-list" ref="container">
    <div 
      class="virtual-list-content"
      :style="{ height: totalHeight + 'px' }"
    >
      <div
        v-for="item in visibleItems"
        :key="item.id"
        :style="{ transform: `translateY(${item.offset}px)` }"
        class="virtual-list-item"
      >
        {{ item.content }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted, onUnmounted } from 'vue'

export default {
  props: {
    items: {
      type: Array,
      required: true
    },
    itemHeight: {
      type: Number,
      default: 50
    }
  },
  
  setup(props) {
    const container = ref(null)
    const scrollTop = ref(0)
    
    const totalHeight = computed(() => props.items.length * props.itemHeight)
    
    const visibleItems = computed(() => {
      const containerHeight = container.value?.clientHeight || 0
      const startIndex = Math.floor(scrollTop.value / props.itemHeight)
      const endIndex = Math.min(
        startIndex + Math.ceil(containerHeight / props.itemHeight) + 1,
        props.items.length
      )
      
      return props.items.slice(startIndex, endIndex).map((item, index) => ({
        ...item,
        offset: (startIndex + index) * props.itemHeight
      }))
    })
    
    const handleScroll = (event) => {
      scrollTop.value = event.target.scrollTop
    }
    
    onMounted(() => {
      container.value?.addEventListener('scroll', handleScroll)
    })
    
    onUnmounted(() => {
      container.value?.removeEventListener('scroll', handleScroll)
    })
    
    return {
      container,
      totalHeight,
      visibleItems
    }
  }
}
</script>
```

## 🛠️ Ecosystem & Tools

### Vue CLI & Vite
Build tools cho Vue.js development:

```javascript
// vue.config.js
module.exports = {
  publicPath: process.env.NODE_ENV === 'production' ? '/app/' : '/',
  
  configureWebpack: {
    optimization: {
      splitChunks: {
        chunks: 'all',
        cacheGroups: {
          vendor: {
            test: /[\\/]node_modules[\\/]/,
            name: 'vendors',
            chunks: 'all'
          }
        }
      }
    }
  },
  
  chainWebpack: config => {
    config.plugin('html').tap(args => {
      args[0].title = 'My Vue App'
      return args
    })
  }
}

// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['vue', 'vue-router'],
          utils: ['lodash', 'axios']
        }
      }
    }
  },
  
  server: {
    port: 3000,
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true
      }
    }
  }
})
```

### Testing với Vue Test Utils
Testing Vue.js components:

```javascript
// Counter.test.js
import { mount } from '@vue/test-utils'
import Counter from '../components/Counter.vue'

describe('Counter', () => {
  test('renders initial count', () => {
    const wrapper = mount(Counter)
    expect(wrapper.text()).toContain('Count: 0')
  })
  
  test('increments count when button is clicked', async () => {
    const wrapper = mount(Counter)
    const button = wrapper.find('button')
    
    await button.trigger('click')
    
    expect(wrapper.text()).toContain('Count: 1')
  })
  
  test('emits increment event', async () => {
    const wrapper = mount(Counter)
    const button = wrapper.find('button')
    
    await button.trigger('click')
    
    expect(wrapper.emitted('increment')).toBeTruthy()
    expect(wrapper.emitted('increment')).toHaveLength(1)
  })
})

// Testing composables
import { renderComposable } from '@vue/test-utils'
import { useCounter } from '../composables/useCounter'

test('useCounter composable', () => {
  const { result } = renderComposable(() => useCounter(5))
  
  expect(result.count.value).toBe(5)
  expect(result.doubleCount.value).toBe(10)
  
  result.increment()
  expect(result.count.value).toBe(6)
  expect(result.doubleCount.value).toBe(12)
})
```

## ❓ Common Interview Questions

### 1. Vue 2 vs Vue 3
**Q: Sự khác biệt chính giữa Vue 2 và Vue 3?**

**A:** 
- **Composition API**: Logic organization, better reusability
- **Performance**: Smaller bundle, faster rendering, tree-shaking
- **TypeScript**: Better TypeScript support
- **Multiple root nodes**: Fragments support
- **Teleport**: Render content outside component tree
- **Suspense**: Async component handling

### 2. Reactivity System
**Q: Vue.js reactivity system hoạt động như thế nào?**

**A:** 
- **Proxy-based**: Vue 3 sử dụng ES6 Proxy
- **Getter/Setter**: Vue 2 sử dụng Object.defineProperty
- **Dependency tracking**: Tự động track dependencies
- **Batch updates**: Batched DOM updates
- **Deep reactivity**: Nested object reactivity

### 3. Component Communication
**Q: Các cách components giao tiếp trong Vue.js?**

**A:** 
- **Props down**: Parent → Child
- **Events up**: Child → Parent ($emit)
- **Provide/Inject**: Dependency injection
- **Event bus**: Global event system
- **Vuex/Pinia**: Centralized state management
- **Props validation**: Type checking, required fields

### 4. Performance Optimization
**Q: Cách optimize Vue.js application performance?**

**A:** 
- **Lazy loading**: Route-based, component-based
- **Virtual scrolling**: Large list optimization
- **Memoization**: Computed properties, v-memo
- **Tree shaking**: Unused code elimination
- **Code splitting**: Dynamic imports
- **Bundle optimization**: Webpack/Vite configuration

### 5. Testing Strategy
**Q: Testing strategy cho Vue.js application?**

**A:** 
- **Unit testing**: Component logic, composables
- **Integration testing**: Component interactions
- **E2E testing**: User workflows
- **Vue Test Utils**: Component testing utilities
- **Vitest**: Fast unit testing
- **Cypress**: E2E testing

## 📚 Resources
- [Vue.js Official Documentation](https://vuejs.org/)
- [Vue 3 Migration Guide](https://v3-migration.vuejs.org/)
- [Composition API Guide](https://vuejs.org/guide/extras/composition-api-faq.html)
- [Vue Test Utils](https://test-utils.vuejs.org/)
- [Pinia Documentation](https://pinia.vuejs.org/) 