<script setup lang="ts">
import { ref, watch, toValue } from 'vue'

const headers = new Headers({
	'Content-Type': 'application/json',
	'x-api-key': import.meta.env.VUE_APP_API_CATS_TOKEN,
})

const requestOptions: RequestInit = {
	method: 'GET',
	headers: headers,
	redirect: 'follow' as RequestRedirect,
}

const baseURL =
	'https://api.thecatapi.com/v1/images/search?size=med&mime_types=jpg&format=json&has_breeds=true&order=RANDOM&page=0&limit=1'
const catURL = ref(baseURL)

function useFetch(urlRef: typeof catURL) {
	const data = ref<{ url: string }[] | null>(null)
	const error = ref(null)

	const fetchData = () => {
		data.value = null
		error.value = null

		fetch(toValue(urlRef), requestOptions)
			.then(res => res.json())
			.then(json => {
				data.value = json
			})
			.catch(err => {
				error.value = err
				console.error('Fetch error:', err)
			})
	}

	// Trigger fetchData immediately on mount and on any change to urlRef
	watch(urlRef, fetchData, { immediate: true })

	return { data, error }
}

function reFetch() {
	// Append a unique parameter to the URL to force re-fetch
	catURL.value = `${baseURL}&timestamp=${Date.now()}`
}

const { data, error } = useFetch(catURL)
</script>

<template>
	<div class="cats">
		<button @click="reFetch">New Cat</button>

		<p v-if="error">Error: {{ error }}</p>
<<<<<<< HEAD
		<!-- <img v-else-if="data" alt="Gif" :src="data[0].url" /> -->
		<img v-else-if="data" alt="Gif" :src="data[0].url" />
=======
		<img v-else-if="data" alt="Cat images" :src="data[0].url" />
>>>>>>> 01c30e2 (feat: my first project in vue)
		<p v-else>Loading...</p>
	</div>
</template>

<style lang="scss" scoped>
@use './ImageCats.scss';
</style>
