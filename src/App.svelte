<script lang="ts">
	import { cubicOut } from "svelte/easing";
	import { crossfade } from "svelte/transition";
	import { flip } from "svelte/animate";
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

	const [send, receive] = crossfade({
		duration: (d) => Math.sqrt(d * 200),

		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === "none" ? "" : style.transform;

			return {
				duration: 300,
				easing: cubicOut,
				css: (t) => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`,
			};
		},
	});
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
					if (e.key === "Enter") addTodoItem();
				}}
			/>

			<select
				class="form bottomInput"
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

			<button class="form bottomInput" on:click={addTodoItem}>
				Add Todo
			</button>
		</div>

		{#each allTodos as todo, index (todo)}
			<div
				in:receive={{ key: todo }}
				out:send={{ key: todo }}
				animate:flip={{ duration: 700 }}
				class:lineTodo={index != 0}
				class="allTodos"
			>
				<TodoItem
					name={todo.name}
					todoType={todo.todoType}
					on:delete={() => {
						allTodos.splice(index, 1);
						allTodos = allTodos;
					}}
				/>
			</div>
		{/each}
	</div>
	<div class="footer">Made by Felix</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 500px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	.fullForm {
		margin-bottom: 2em;
	}

	.form {
		padding: 0.7em;
		font-size: large;
		border-radius: 7px;
		margin: 0 0.2em;
		font-weight: 600;
	}

	button {
		color: #fff;
		background: #ff3e00;
		display: inline-block;
		border: none;
		transition: all 0.2s ease 0s;
	}

	.allTodos {
		max-width: 400px;
		margin: auto;
	}

	.lineTodo {
		border-top: 1px solid #cccccc;
	}

	.footer {
		bottom: 0;
		left: 0px;
		right: 0px;
		width: 100%;
		position: fixed;
		padding-bottom: 1em;
		font-weight: 600;
		color: rgb(109, 109, 109);
		background-color: #fff;
	}

	@media (max-width: 640px) {
		main {
			max-width: none;
		}
		.bottomInput {
			margin-top: 1em;
		}
	}
</style>
