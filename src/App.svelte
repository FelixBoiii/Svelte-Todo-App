<script lang="ts">
	import TodoItem, { TodoTypeTulip } from "./TodoItem.svelte";
	type TodoTypeOptions = 0 | 1 | 2 | 3;

	let allTodos: Array<Todo> = [];
	let newTodoName: string = "";
	let newTodoType: TodoTypeOptions = 0;

	class Todo {
		name: string;
		todoType: TodoTypeOptions;

		constructor(name: string, todoType: TodoTypeOptions) {
			this.name = name;
			this.todoType = todoType;
		}

		edit(
			name: string = this.name,
			todoType: TodoTypeOptions = this.todoType
		) {
			this.name = name;
			this.todoType = todoType;
		}
	}

	const addTodoItem = () => {
		allTodos.push(new Todo(newTodoName, newTodoType));
		allTodos = allTodos;
		newTodoName = "";
	};
</script>

<main>
	<h1>Todo App</h1>
	<div>
		<input
			type="text"
			bind:value={newTodoName}
			on:keypress={(e) => {
				if (e.charCode === 13) addTodoItem();
			}}
		/>

		<select name="todoType" id="todoTypeInput" bind:value={newTodoType}>
			{#each TodoTypeTulip.map((x) => x[0]) as TodoTypeValue, index}
				<option value={index}>
					{TodoTypeValue}
				</option>
			{/each}
		</select>

		<button
			on:click={() => {
				allTodos.push(new Todo(newTodoName, newTodoType));
				allTodos = allTodos;
				newTodoName = "";
			}}
		>
			Add Todo
		</button>

		{#each allTodos as todo, index}
			<TodoItem
				name={todo.name}
				todoType={todo.todoType}
				on:delete={() => {
					allTodos.splice(index, 1);
					allTodos = allTodos;
				}}
			/>
		{/each}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
