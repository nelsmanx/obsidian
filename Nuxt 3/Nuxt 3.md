## New project

```bash
npx nuxi@latest init <project-name>
```

## Development Server

```bash
npm run dev -- -o
```
(`-- -o` A browser window should automatically open for [http://localhost:3000](http://localhost:3000/).)



# Routing

 Every Vue file inside the `~/pages/` directory creates a route that displays the contents of the file. Nuxt routing is based on [vue-router](https://router.vuejs.org/) and generates the routes from every component created in the  `~/pages` directory , based on their filename.
 
```
-| pages/
---| about.vue
---| index.vue
---| products/
-----| index.vue
```

The `pages/index.vue` file will be mapped to the `/` route of your application.
The `pages/products/index.vue` file will be mapped to the `/products/` route.


## Dynamic Routes

If you place anything within square brackets, it will be turned into a dynamic route parameter. 

```
-| pages/
---| products/
-----| [id].vue
```


## Route Parameters

Accessing the current route details
```vue
<template>
	<div>
		<p>Products details for {{ route.params.id }}</p>
		<p>Products details for {{ id }}</p>

		<!-- via the $route object -->
		<p>Products details for {{ $route.params.id }}</p>
	</div>
</template>

<script setup>
const route = useRoute();
console.log(route.params.id); // When accessing /products/1, will be 1
// or
const { id } = useRoute().params;
console.log(id);
</script>
```


## Navigation

The `<NuxtLink>` component links pages between them. It renders an `<a>` tag with the `href` attribute set to the route of the page. Once the application is hydrated, page transitions are performed in JavaScript by updating the browser URL. <u>This prevents full-page refreshes</u> and allows for animated transitions.
Active links have classes `"router-link-active"` and `"router-link-exact-active"`

```vue
<header>
    <nav>
      <ul>
        <li><NuxtLink to="/about">About</NuxtLink></li>
        <li><NuxtLink to="/posts/1">Post 1</NuxtLink></li>
        <li><NuxtLink to="/posts/2">Post 2</NuxtLink></li>
      </ul>
    </nav>
</header>
```


## Layouts

 Layouts are Vue files using `<slot />` components to display the page content. The `layouts/default.vue` file will be used by default. 
 
```
-| layouts/
---| default.vue
---| custom.vue
```

```vue
<template>
  <div>
    <p>Some default layout content shared across all pages</p>
    <slot />
  </div>
</template>
```

You can use the `custom` layout in your page:

```vue
<script setup>
	definePageMeta({
	  layout: 'custom'
	})
</script>
```


## Adding Tailwind module

1. Add `@nuxtjs/tailwindcss` dependency to your project
```bash
npm install --save-dev @nuxtjs/tailwindcss
```

2.  Add `@nuxtjs/tailwindcss` to the `modules` section of `nuxt.config.{ts,js}`
```js
export default defineNuxtConfig({
	modules: ['@nuxtjs/tailwindcss']
})
```



## Data fetching

Nuxt can fire this code on a server and in a browser. It can prerender page on a server using this code and inject it in a template. Once we have this page in a browser and we navigate to another page, nuxt will fire this code in a browser.  It matters, because not all code can be used on both a server and a browser. E.g `window` object  will cause an error on a server. 

```vue
<template>
	<div>
		<h2>Products</h2>
		<ul>
			<li v-for="{ id, title, description, price } in products">
				<NuxtLink :to="`/products/${id}`">{{ title }}</NuxtLink>
				<p>{{ description }}</p>
				<p>{{ price }}</p>
			</li>
		</ul>
	</div>
</template>

<script setup>
const { data: products } = await useFetch('https://fakestoreapi.com/products');
</script>
```

`useFetch` use keys to prevent refetching the same data. `useFetch` uses the provided URL as a key. Alternatively, a key value can be provided in the options object passed as a last argument.

```vue
<script setup>
	const { data: product } = await useFetch(url, {key: id});
</script>
```


## Error Handling

Customize the default error page by adding `~/error.vue` in the source directory of your application.
`clearError` function will clear the currently handled Nuxt error. It also takes an optional path to redirect to (for example, if you want to navigate to a 'safe' page).

```vue
<template>
	<div>
		<h2>{{ error.statusCode }}</h2>
		<p>{{ error.message }}</p>
		<button @click="errorHandler">Home</button>
	</div>
</template>

<script setup>
	const props = defineProps(['error']);
	const errorHandler = () => clearError({ redirect: '/' });
</script>
```
### createError

If you throw an error created with `createError` on server-side, it will trigger a full-screen error page which you can clear with `clearError`

```vue
<script setup>
const { id } = useRoute().params;
const { data: product } = await useFetch(`https://fakestoreapi.com/products/${id}`);

if (!product.value) {
	throw createError({
		statusCode: 404,
		statusMessage: 'Product not found'
	});
}
</script>
```

On client-side, it will throw a non-fatal error for you to handle. If you need to trigger a full-screen error page, then you can do this by setting `fatal: true`.

```vue
/pages/products/index
<template>
	<NuxtLink to="/products/fakeId">Fake link</NuxtLink>
</template>
```

```vue
/pages/products/[id].vue
<script setup>
if (!product.value) {
	throw createError({
		statusCode: 404,
		statusMessage: 'Product not found',
		fatal: true
	});
}
</script>
```


## SEO and Meta

Out-of-the-box, Nuxt provides sane defaults, which you can override if needed.
```js
export default defineNuxtConfig({
  app: {
    head: {
      charset: 'utf-8',
      viewport: 'width=device-width, initial-scale=1',
    }
  }
})
```

We can add custom meta tags over here
```js
export default defineNuxtConfig({
  devtools: { enabled: true },
  app: {
	head: {
		meta: [
			{name: 'yandex-verification', content: 'fd5d3xxxx122e'}
		]
	}
  }
})
```

### useHead

The `useHead` composable function allows you to manage your head tags in a programmatic and ==reactive== way

```vue
<script setup>
useHead({
	title: 'Products page',
	meta: [
		{ name: 'description', content: 'description of products page' }
	]
});
</script>
```

### useSeoMeta

The `useSeoMeta` composable lets you define your site's SEO meta tags as a flat object

```vue
<script setup>
useSeoMeta({
	title: 'Products page',
	description: 'Description of products page'
});
</script>
```


## Components

```vue
<template>
	<Head>
		<Title>{{ product.title }}</Title>
		<Meta name="description" :content="product.description" />
	</Head>
</template>

<script setup>
	const { data: product } = await useFetch('https://fakestoreapi.com/products/1');
</script>
```


### Pnpm install error 

```bash
rm -rf node_modules  

rm pnpm-lock.yaml 

pnpm store prune 

pnpm install
```
