# [Nuxt.js](https://nuxt.com)

Easy to learn. Easy to master
Learn everything you need to know, from beginner to master.

易学易精通
从初学者到专家，学会您所需的一切。

Nuxt.js是一个基于Vue.js的通用应用程序框架，它可以帮助开发者构建单页应用（SPA）和服务器渲染应用（SSR）。Nuxt.js提供了一些默认配置和约定，使得Vue.js应用程序的开发更加简单和高效。

```javascript
yarn create nuxt-app <project-name>
cd <project-name>
yarn dev
```
访问 http://localhost:3000。Well done！

## 特点和功能：

### 服务器端渲染（SSR）：
Nuxt.js支持服务器端渲染，可以将Vue.js应用程序在服务器上预渲染，并将生成的静态HTML直接返回给客户端。这提供了更好的首次加载性能和搜索引擎优化（SEO）。

### 自动路由配置：
Nuxt.js根据文件系统的约定自动生成路由配置，无需手动编写和维护路由。你只需在特定的目录下创建Vue组件，Nuxt.js会自动为你生成相应的路由。

文件结构
```javascript
pages/
--| user/
-----| index.vue
-----| one.vue
--| index.vue
```
自动生成的路由
```javascript
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'user',
      path: '/user',
      component: 'pages/user/index.vue'
    },
    {
      name: 'user-one',
      path: '/user/one',
      component: 'pages/user/one.vue'
    }
  ]
}
```
### 动态路由
有时候我们无法预先知道路由的名称，比如当我们调用一个 API 来获取用户列表或博客文章时。这些被称为动态路由。要创建一个动态路由，你需要在 .vue 文件名或目录名之前加上下划线（_）。你可以随意命名文件或目录，但必须以下划线作为前缀。

文件结构
```javascript
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```
生成的路由
```javascript
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'users-id',
      path: '/users/:id?',
      component: 'pages/users/_id.vue'
    },
    {
      name: 'slug',
      path: '/:slug',
      component: 'pages/_slug/index.vue'
    },
    {
      name: 'slug-comments',
      path: '/:slug/comments',
      component: 'pages/_slug/comments.vue'
    }
  ]
}
```

### 预处理器支持：
Nuxt.js支持多种CSS预处理器，如Sass、Less和Stylus，以及JavaScript预处理器，如TypeScript。

### 数据获取：
Nuxt.js提供了一种简单的方式来获取应用程序所需的数据。你可以在页面组件中使用asyncData方法来获取异步数据，或者使用fetch方法来获取数据并填充Vuex store。
```javascript
<template>
  <div>
    <p v-if="$fetchState.pending">Fetching mountains...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>
    <div v-else>
      <h1>Nuxt Mountains</h1>
      <ul>
        <li v-for="mountain of mountains">{{ mountain.title }}</li>
      </ul>
      <button @click="$fetch">Refresh</button>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        mountains: []
      }
    },
    async fetch() {
      this.mountains = await fetch(
        'https://api.nuxtjs.dev/mountains'
      ).then(res => res.json())
    }
  }
</script>
```
### 插件系统：
Nuxt.js提供了插件系统，可以方便地集成第三方库、添加全局样式和扩展Vue.js的功能。

### 静态站点生成（SSG）：
除了服务器端渲染，Nuxt.js还支持生成静态站点。你可以通过运行nuxt generate命令将Vue.js应用程序生成为静态HTML文件，这些文件可以部署到任何静态文件托管服务上。
```javascript
export default {
  ssr: true // default value
}
```
与从HTML中获取所有内容不同，我们只获取一个包含JavaScript文件的基本HTML文档，然后使用浏览器来渲染站点的其余部分。
要进行客户端渲染，请将ssr设置为false。


## 元数据标签和SEO
全局配置
```javascript
export default {
  head: {
    title: 'my website title',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      {
        hid: 'description',
        name: 'description',
        content: 'my website description'
      }
    ],
    link: [{ rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }]
  }
}
```


## 总结

总之，Nuxt.js是一个功能强大的Vue.js应用程序框架，它简化了Vue.js应用程序的开发和部署过程，提供了服务器端渲染、自动路由配置、数据获取和插件系统等特性，使得开发者可以更快速地构建出高性能的Vue.js应用程序。





