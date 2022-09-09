
<script>

	import Titulo from "./componenets/Titulo.svelte"
    let texto = "NOTAS"

	let todos = []
	let todo = {id: "", texto: "", estado: false}
	
	if(localStorage.getItem("todos")){
		todos = JSON.parse(localStorage.getItem("todos"))
	}

	$: localStorage.setItem("todos", JSON.stringify(todos))

const addTodo = () => {
	if(!todo.texto.trim()){
		console.log("texto vacio")
		todo.texto = ""
		return
	}
	todo.id = Date.now()
	todos = [...todos, todo]
	console.log(todos)
	todo = {id: "", texto: "", estado: false}



}

const delTodo = id => {
	todos = todos.filter(item => item.id !== id)
} 


const editTodo = id => { 
	todos = todos.map(item => item.id === id?
	{...item, estado: !item.estado}
	: item
	)}


const calssIcono = valor => (
	valor ?  "bi bi-arrow-clockwise" : "bi bi-check2")

const classEstado = valor => valor ? "btn-success" : "btn-warning"
</script>

<main>	
<div class="container">
	<Titulo {texto}/>
	<form on:submit|preventDefault={addTodo}>
		<input type="text"
		       placeholder="enter para agregar algo"
			   class="form-control shadow border-0"
			   bind:value={todo.texto}>
	</form>
	{#each todos as item}
		 <div class="shadow my-3 p-3 lead">
			<p 
			    class={item.estado ? "text-decoration-line-through" : ""}
				>{item.texto}</p>
			<button class="btn btn-sm {classEstado(item.estado)}" on:click={editTodo(item.id)}>
				<i class={calssIcono(item.estado)}></i>
			</button>
			<button class="btn btn-sm btn-danger" on:click={delTodo(item.id)}>
				<i class="bi bi-trash3-fill"></i>
			</button>
		</div>
	{/each}
</div>
		
</main>