<script setup lang="ts">
import { ref, watchEffect, toValue } from 'vue'

const headers = new Headers({
	'Content-Type': 'application/json',
	'x-api-key':
		'live_rAEsQUwaVunMfgWBSHm4FzbQ4IStKyUre9ZdpZB5Wm3xWinphIcQeJmsPyjItnba',
})

const requestOptions = {
	method: 'GET',
	headers: headers,
	redirect: 'follow',
}

const catURL = ref(
	'https://api.thecatapi.com/v1/images/search?size=med&mime_types=jpg&format=json&has_breeds=true&order=RANDOM&page=0&limit=1',
)
function useFetch(url) {
	const data = ref(null)
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

const { data, error } = useFetch(catURL)

function handleNewCat() {
	catURL.value = `https://api.thecatapi.com/v1/images/search?size=med&mime_types=jpg&format=json&has_breeds=true&order=RANDOM&page=0&limit=1&timestamp=${Date.now()}`
}
</script>

<template>
	<div class="cats">
		<button @click="handleNewCat">New Cat</button>

		<p v-if="error">Error: {{ error }}</p>
		<img
			v-else-if="data"
			alt="Gif"
			:src="data[0].url"
			@click="() => (catGif = data[0].url)"
		/>
		<p v-else>Loading...</p>
	</div>
</template>

<style lang="scss" scoped>
@use './ImageCats.scss';
</style>
