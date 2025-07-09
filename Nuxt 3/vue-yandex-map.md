```bash
npm install vue-yandex-maps@beta
```


```js
// project/plugins/ymapPlugin.client.js

import plugin from 'vue-yandex-maps';
import { defineNuxtPlugin } from 'nuxt/app';

const settings = {
	apiKey: '',
	lang: 'ru_RU',
	coordorder: 'latlong',
	debug: true,
	version: '2.1'
};

export default defineNuxtPlugin((nuxtApp) => {
	nuxtApp.vueApp.use(plugin, settings);
});
```

```html

<script setup>

const coordinates = [55.779054, 37.588611];
const markerOptions = {
	iconLayout: "default#imageWithContent",
	iconImageHref: "/img/logo-map.png",
	iconImageSize: [90, 90],
	iconImageOffset: [-20, -32],
};

</script>

<div class="maps" id="map">
	<ClientOnly>
		<YandexMap 
			:coordinates="coordinates" 
			:zoom="15" 
			:controls="[]" 
			:behaviors="['drag', 'dblClickZoom', 'multiTouch']"
		>
			<YandexMarker 
				:coordinates="coordinates" 
				:options="markerOptions" 
				marker-id="yandex-balloon" 
			/>
		</YandexMap>
	</ClientOnly>
</div>
```