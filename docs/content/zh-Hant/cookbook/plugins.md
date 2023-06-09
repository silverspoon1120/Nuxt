---
title: 擴充元件
position: 24
description: 'Nuxt.js 的 Typescript 支援'
category: '更多使用方式'
---

## I. 注入到 Vue 實例

### Plugin

```ts
import Vue from 'vue'

declare module 'vue/types/vue' {
  interface Vue {
    $myInjectedFunction(message: string): void
  }
}

Vue.prototype.$myInjectedFunction = (message: string) => console.log(message)
```

### 使用方法

```html
<template>
  <div>
    <button @click="$myInjectedFunction()">Click me !</button>
    <button @click="someMethod">Click me !</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  mounted () {
    this.$myInjectedFunction('works in mounted')
  }
})
</script>
```

## II. 注入到 context

### Plugin

```ts
import { Plugin } from '@nuxt/types'

declare module '@nuxt/types' {
  interface Context {
    $myInjectedFunction(message: string): void
  }
}

const myPlugin: Plugin = (context) => {
  context.$myInjectedFunction = (message: string) => console.log(message)
}

export default myPlugin
```

### 使用方法

```html
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  asyncData (context) {
    context.$myInjectedFunction('works in asyncData')
  }
})
</script>
```

## III. 兩者一起注入

### Plugin

```ts
import { Plugin } from '@nuxt/types'

declare module 'vue/types/vue' {
  interface Vue {
    $myInjectedFunction(message: string): void
  }
}

declare module '@nuxt/types' {
  interface NuxtAppOptions {
    $myInjectedFunction(message: string): void
  }
  // nuxtContext.$myInjectedFunction
  interface Context {
    $myInjectedFunction(message: string): void
  }
}

declare module 'vuex/types/index' {
  interface Store<S> {
    $myInjectedFunction(message: string): void
  }
}

const myPlugin: Plugin = (context, inject) => {
  inject('myInjectedFunction', (message: string) => console.log(message))
}

export default myPlugin
```

### 使用方法

```html
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  mounted () {
    this.$myInjectedFunction('works in mounted')
  },
  asyncData (context) {
    context.app.$myInjectedFunction('works in asyncData')
  }
})
</script>
```
