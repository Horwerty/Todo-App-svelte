<script>
	import { onMount } from "svelte";
	import { fly} from 'svelte/transition' 
	let newTodo = '';
	let tempId = 4
	// let yes = false;
	let currentFilter = 'all';
	let quoteZone;

	let todos = []

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

		function empTee(){
			alert("Kindly input your to do")
		}

		function updateFilter(filter){
			currentFilter = filter;

		}

		function editTodo(todo){
			todo.editing = true;
			todos = todos
		}

		function doneEditing(todo){
			todo.editing = false;
			todos = todos
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

		onMount(async() => {
			const res = await fetch('https://api.kanye.rest')
			const response = await res.json();
			quoteZone= response.quote;
		})

		
		$: todoRemainings = todos.filter(todo => !todo.completed).length

		$: filteredTodos = currentFilter === 'all'
		 ? todos
		 : currentFilter === 'completed'
		   ? todos.filter(todo => todo.completed)
		   : todos.filter(todo => !todo.completed);
</script>

<main>
		<div>
			<h2>TO DO APP</h2>
			<div class="inputZone">
				<input type="search" bind:value={newTodo} class="todo-input" >
				{#if newTodo == ""}
						<button on:click={empTee} class="addBtn">Add</button>
				{:else}
				<button on:click={addTodo} class="addBtn">Add</button>
				{/if}
			</div>
		</div>
		{#each filteredTodos as todo}
			<div class="todo-item">
				<div class="todo-item-left" transition:fly={{y:20, duration: 300}}>
					<input type="checkbox" bind:checked={todo.completed} >
					{#if !todo.editing}
						<!-- svelte-ignore a11y-no-static-element-interactions -->
						<div class="todo-item-label" class:completed={todo.completed}
						on:dblclick={() => editTodo(todo)}>{todo.title}</div>
					{:else}
						<!-- svelte-ignore a11y-autofocus -->
						<input type="text" class="todo-item-edit" bind:value={todo.title} on:blur={() => doneEditing(todo)}>
					{/if}	
				</div>
				<div class="remove-item" >
					<!-- <button on:click={() => deleteTodo(todo.id )}>&times;</button> -->
					<button on:click={deleteTodo(todo.id)}>&times;</button>
				</div>
			</div>
		{/each}
		
		<div class="extra-container">
			<div><label><input type="checkbox" on:click={checkAllTodos}>Check All</label></div>
			<div class="counterTxt">{todoRemainings} item(s) left</div>
		</div>
		<div class="extra-container">
			<div class="filterBtn">
				<button on:click={() => updateFilter('all')} class:active={currentFilter=== 'all'}>All</button>
				<button on:click={() => updateFilter('active')} class:active={currentFilter=== 'active'}>Active</button>
				<button on:click={() => updateFilter('completed')} class:active={currentFilter=== 'completed'}>Completed</button>
			</div>
			<div>
				<button on:click={clearCompleted} class="clrBtn">Clear Completed</button>
			</div>
		</div>
		<p class="quotZee">{quoteZone}</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		width: 480px;
		margin: 0 auto;
		border: 2px black solid;
		background-color: gray;
	}

	h2{
		font-weight: bold;
		color: #fff;
	}

	.counterTxt{
		font-weight: bold;
		color: #fff;
	}

	.inputZone{
		display: flex;
		justify-content: space-between;
	}

	.quotZee{
		color: #fff;
		font-weight: bold;
	}

	.todo-item{
		font-weight: 600;
		color: #fff;
	}
	.remove-item button{
		width: 2.5rem;
		height: 2rem;
		background-color: orangered;
		font-weight: bold;
		border: none;
		cursor: pointer;
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

	.addBtn{
		width: 4.5rem;
		border-radius: 5px;
		border: none;
		height: 3rem;
		font-weight: bold;
		cursor: pointer;
	}

	.addBtn:hover {
		background-color: green;
		color: #fff;
	}

	

	.clrBtn{
		width: 9rem;
		border-radius: 5px;
		border: none;
		height: 2.8rem;
		font-weight: bold;
		cursor: pointer;
	}

	.clrBtn:hover {
		background-color: orangered;
		color: #fff;
	}

	.todo-item{
		display: flex;
		margin-bottom: 12px;
		align-items: center;
		justify-content: space-between;
	}

	.active {
		
		cursor: pointer;
	}

	.active:hover{
		background-color: brown;
	}

	.todo-item-left, .extra-container{
		display: flex;
		justify-content: space-between;
	}

	.filterBtn button{
		width: 5.9rem;
		border-radius: 5px;
		border: none;
		height: 2.8rem;
		font-weight: bold;
		cursor: pointer;
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