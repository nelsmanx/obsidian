
As shown in the [Nuxt docs](https://nuxtjs.org/guide/routing/), a route name is automatically generated for each page in the application. For example, the page found at `pages/user/index.vue` will automatically be given the `name` of `‘user’`. That name can then be used in a `<nuxt-link>`, like so:

```javascript
<nuxt-link :to=”{ name: ‘user’ }” />
```

If the route requires parameters, then a `params` object can be included. For example, a link to the `pages/user/_id/edit.vue` page would look like the following:

```javascript
<nuxt-link :to=”{ name: ‘user-id-edit’, params: { id: user.id } }” />
```

The issue with this method is that the names **cannot** be chosen by the developer, and if the directory structure is changed, then the route name will change accordingly and links will have to be updated.

```js
<NuxtLink :to="{
	name: 'products-category',
	params: { category: category.slug },
	query: { subcategory: subcategory.slug }
}">
```