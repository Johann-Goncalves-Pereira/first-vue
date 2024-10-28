<script setup lang="ts">
import { ref, reactive } from 'vue'

const input = ref('')

const todos = reactive([
	{ id: 1, text: 'Learn HTML' },
	{ id: 2, text: 'Learn JavaScript' },
	{ id: 3, text: 'Learn Vue' },
])

function handleSubmit(element: HTMLFormElement) {
	element.preventDefault()
}

function handleInput(event: InputEvent) {
	const text = (event.target as HTMLInputElement).value

	input.value = text
}

function handleAddTodo() {
	if (!input.value) return

	todos.push({
		id: todos.length + 1,
		text: input.value,
	})

	input.value = ''
}

function handleRemoveTodo(idByIndex: number) {
	todos.splice(todos.indexOf(idByIndex), 1)
}
</script>

<template>
	<form method="dialog" @submit="handleSubmit($event)">
		<div class="control">
			<input
				type="text"
				autofocus
				:value="input"
				@input="handleInput($event)"
			/>
			<button :class="{ disabled: !input }" @click="handleAddTodo">
				Add Todo
			</button>
		</div>
		<ul class="list">
			<li v-for="todo in todos" :key="todo.id">
				{{ todo.text }}

				<button type="button" @click="handleRemoveTodo(todo)">Remove</button>
			</li>
		</ul>
	</form>
</template>

<style lang="scss" scoped>
@use './TodoList.scss';
</style>
