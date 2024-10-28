<script setup lang="ts">
import VueApp from './components/VueApp.vue'
import { reactive } from 'vue'

const light = reactive({ x: 0, y: 0 })
const isSafari = /^((?!chrome|android).)*safari/i.test(navigator.userAgent)

// Update light position based on mouse position
window.addEventListener('mousemove', (ev: MouseEvent) => {
	const el = document.querySelector('.light') as HTMLElement

	if (el) {
		light.x = ev.clientX - el.offsetWidth * 0.5
		light.y = ev.clientY - el.offsetHeight * 0.5
	}
})
</script>

<template>
	<main class="main">
		<VueApp />
	</main>
	<div
		class="light"
		:style="{
			transform: `translate3d(${light.x}px, ${light.y}px,0)`,
			transitionTimingFunction: isSafari
				? 'cubic-bezier(0, 0.54, 0.49, 0.71)'
				: 'ease-out',
		}"
	/>
</template>

<style lang="scss" scoped>
@use './App.scss';
</style>
