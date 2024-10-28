<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue'

import { X, Minus, Expand } from 'lucide-vue-next'
import TodoList from './TodoList/TodoList.vue'
import ImageCats from './ImageCats/ImageCats.vue'

const windowRef = ref<HTMLDialogElement | null>(null)
const expand = ref(false)
const tab = ref<'todo' | 'cats'>('todo')

onMounted(() => {
	windowRef.value = document.getElementById(
		'vue-window-app',
	) as HTMLDialogElement | null
})

function handleShowModal() {
	nextTick(() => {
		if (windowRef.value) {
			windowRef.value.showModal()
		}
	})
}

function handleCloseDialogOutOfBound(
	event: MouseEvent,
	element: HTMLDialogElement,
) {
	if (element instanceof HTMLDialogElement) {
		const dimensions = element.getBoundingClientRect()

		if (
			event.clientX < dimensions.left ||
			event.clientX > dimensions.right ||
			event.clientY < dimensions.top ||
			event.clientY > dimensions.bottom
		) {
			element.close()
			event.preventDefault()
		}
	}
}

function handleCloseDialog(event: Event) {
	if (windowRef.value) {
		windowRef.value.close()
		event.preventDefault()
	}
}

function handleTab(tabName: 'todo' | 'cats') {
	tab.value = tabName
}
</script>

<template>
	<button class="vue-window__button" @click="handleShowModal">
		<img
			alt="Vue logo"
			class="logo"
			src="../../assets/logo.svg"
			width="125"
			height="125"
		/>
	</button>
	<dialog
		class="vue-window__dialog"
		:class="{ expand: expand }"
		id="vue-window-app"
		@click="
			handleCloseDialogOutOfBound($event, $event.target as HTMLDialogElement)
		"
	>
		<header class="header">
			<div class="header__window-control">
				<button class="button" @click="handleCloseDialog($event)"><X /></button>
				<button class="button" @click="handleCloseDialog($event)">
					<Minus />
				</button>
				<button class="button" @click="() => (expand = !expand)">
					<Expand />
				</button>
			</div>

			<div class="header__tabs">
				<button
					class="button"
					:class="{ active: tab === 'todo' }"
					@click="handleTab('todo')"
				>
					Todo List
				</button>
				<button
					class="button"
					:class="{ active: tab === 'cats' }"
					@click="handleTab('cats')"
				>
					Random Cats
				</button>
			</div>
		</header>

		<TodoList v-if="tab === 'todo'" />
		<ImageCats v-else-if="tab === 'cats'" />
		<div v-else>error</div>
	</dialog>
</template>

<style lang="scss" scoped>
@use './VueWindow.scss';
</style>
