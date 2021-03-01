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
		if (newTodoName != "") {
			allTodos.push(new Todo(newTodoName, newTodoType));
			allTodos = allTodos;
			newTodoName = "";
		}
	};
</script>

<main>
	<h1>Todo App</h1>
	<div>
		<div class="fullForm">
			<input
				class="form"
				type="text"
				bind:value={newTodoName}
				on:keypress={(e) => {
					if (e.charCode === 13) addTodoItem();
				}}
			/>

			<select
				class="form"
				name="todoType"
				id="todoTypeInput"
				bind:value={newTodoType}
			>
				{#each TodoTypeTulip.map((x) => x[0]) as TodoTypeValue, index}
					<option value={index}>
						{TodoTypeValue}
					</option>
				{/each}
			</select>

			<button class="form" on:click={addTodoItem}> Add Todo </button>
		</div>

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

	button {
		color: #fff;
		background: #ff3e00;
		display: inline-block;
		border: none;
		transition: all 0.2s ease 0s;
	}

	.form {
		padding: 0.7em;
		font-size: large;
		border-radius: 7px;
		margin: 0 0.2em;
	}

	.fullForm {
		margin-bottom: 2em;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
