<script setup lang="ts">
import { ref, watchEffect, toValue } from 'vue'

const headers = new Headers({
	'Content-Type': 'application/json',
	'x-api-key': import.meta.env.VUE_APP_API_CATS_TOKEN,
})

const requestOptions: RequestInit = {
	method: 'GET',
	headers: headers,
	redirect: 'follow' as RequestRedirect,
}

const catURL = ref(
	'https://api.thecatapi.com/v1/images/search?size=med&mime_types=jpg&format=json&has_breeds=true&order=RANDOM&page=0&limit=1',
)

function useFetch(url: string) {
	const data = ref<{ url: string }[] | null>(null)
	const error = ref(null)

	const fetchData = () => {
		// reset state before fetching..
		data.value = null
		error.value = null

		fetch(toValue(url), requestOptions)
			.then(res => res.json())
			.then(json => (data.value = json))
			.catch(err => (error.value = err))
	}

	watchEffect(() => {
		fetchData()
	})

	return { data, error }
}

const { data, error } = useFetch(catURL.value)

function handleNewCat() {
	try {
		if (!catURL.value) {
			throw new Error('Cat URL is not defined')
		}
		catURL.value = `https://api.thecatapi.com/v1/images/search?size=med&mime_types=jpg&format=json&has_breeds=true&order=RANDOM&page=0&limit=1&timestamp=${Date.now()}`
		const { data, error } = useFetch(catURL.value)
		if (error.value) {
			console.error('Error fetching new cat image:', error.value)
		} else if (!data.value || data.value.length === 0) {
			console.warn('No data received from cat API')
		}
	} catch (err) {
		console.error('Error in handleNewCat:', err)
	}
}
</script>

<template>
	<div class="cats">
		<button @click="handleNewCat">New Cat</button>

		<p v-if="error">Error: {{ error }}</p>
		<!-- <img v-else-if="data" alt="Gif" :src="data[0].url" /> -->
		<img v-else-if="data" alt="Gif" :src="data[0].url" />
		<p v-else>Loading...</p>
	</div>
</template>

<style lang="scss" scoped>
@use './ImageCats.scss';
</style>
