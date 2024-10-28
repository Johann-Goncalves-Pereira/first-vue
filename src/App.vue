<script setup lang="ts">
import VueApp from './components/VueApp.vue'
import { reactive } from 'vue'

const light = reactive({ x: 0, y: 0 })

// Update light position based on mouse position
window.addEventListener('mousemove', (ev: MouseEvent) => {
	const el: HTMLElement = document.querySelector('.light')

	light.x = ev.clientX - el.offsetWidth * 0.5
	light.y = ev.clientY - el.offsetHeight * 0.5
})
</script>

<template>
	<main class="main">
		<div
			class="light"
			:style="{ transform: `translate3d(${light.x}px, ${light.y}px,0)` }"
		/>
		<VueApp />
	</main>
</template>

<style lang="scss" scoped>
@use './App.scss';
</style>
