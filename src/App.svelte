<script>
	let newTodo = '';
	let tempId = 4
	// let yes = false;

	let todos = [
		{
			id: 1,
			completed: false,
			title: newTodo,
			editing:false
		},
		// {
		// 	id: 2,
		// 	completed: false,
		// 	title: 'Pen',
		// 	editing:false
		// },
		// {
		// 	id: 3,
		// 	completed: false,
		// 	title: 'Paper',
		// 	editing:false
		// },
	]

		function addTodo(){
			todos.push({
				id: tempId,
				completed: false,
				title: newTodo,
				editing:false
			})
			todos = todos;
			tempId = tempId + 1
			newTodo = '';
		}

		function deleteTodo(id){
			todos = todos.filter(todo => todo.id !== id)
		}

		function clearCompleted(){
			todos = todos.filter(todo => !todo.completed)
		}

		function checkAllTodos(event){
			todos.forEach(todo => todo.completed = event.target.checked);

			todos = todos
		}

		$: todoRemainings = todos.filter(todo => !todo.completed).length
</script>

<main>
		<div>
			<h3>TO DO APP</h3>
			<div class="inputZone">
				<input type="text" bind:value={newTodo} class="todo-input" >
				<button on:click={addTodo}>Add</button>
			</div>
		</div>
		{#each todos as todo}
			<div class="todo-item">
				<div class="todo-item-left">
					<input type="checkbox" bind:checked={todo.completed} >
					<div class="todo-item-label" class:completed={todo.completed}>{todo.title}</div>
				</div>
				<div class="remove-item" >
					<!-- <button on:click={() => deleteTodo(todo.id )}>&times;</button> -->
					<button on:click={deleteTodo(todo.id)}>&times;</button>
				</div>
			</div>
		{/each}
		
		<div class="extra-container">
			<div><label><input type="checkbox" on:click={checkAllTodos}>Check All</label></div>
			<div>{todoRemainings} item(s) left</div>
		</div>
		<div class="extra-container">
			<div>
				<button >All</button>
				<button>Active</button>
				<button>Completed</button>
			</div>
			<div>
				<button on:click={clearCompleted}>Clear Completed</button>
			</div>
		</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.inputZone{
		display: flex;
	}

	/* h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	} */

	.todo-input{
		width: 80%;
		padding: 10px 10px;
		font-size: 18px;
		margin-bottom: 16px;
	}

	.todo-item{
		display: flex;
		margin-bottom: 12px;
		align-items: center;
		justify-content: space-between;
	}

	.todo-item-left, .extra-container{
		display: flex;
		justify-content: space-between;
	}

	.todo-item-label{
		padding-left: 12px;
	}



	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>