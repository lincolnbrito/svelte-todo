<script>
  
  let todoItems = [];
  let newTodo = "";
  let filter = item => item;

  $:filtered = todoItems.filter(filter);
  $:doneTodos = todoItems.filter( item => item.checked).length;
  $:activeTodos = todoItems.filter( item => !item.checked).length;
  
  const addTodo = () => {
    
    if (newTodo.length == 0) return;

    const todo = {
      id: todoItems.length + 1,
      text: newTodo,
      checked: false
    };

    todoItems = [...todoItems, todo];
    newTodo = "";
  }

  const toggleDone = (id) => {
    const index = todoItems.findIndex(item => item.id === Number(id));

    todoItems[index].checked = !todoItems[index].checked;
  }

  const removeTodo = (id) => {
    todoItems = todoItems.filter(item => item.id !== Number(id));
  }

  const clearCompleted = () => {
    todoItems = todoItems.filter( item => !item.checked);
  }

  const filterTodos = (f) => {
    filter = f
  }
 
</script>

 
<main class="container">
  <h1>Svelte Todo</h1>
  
  <form on:submit|preventDefault={addTodo}>
    <input 
      type="text"
      aria-label="Enter a new todo item"
      placeholder="Enter a new todo item"
      bind:value={newTodo} 
    />
  </form>

  <ul class="todo-list">
    {#each filtered as todo (todo.id)}
      <li>
        <label for="todo-{todo.id}">
          <input type="checkbox" id="todo-{todo.id}" bind:checked={todo.checked}> 
          <span class:checked={todo.checked}>
            {todo.text}
          </span>
          <button class="remove" on:click={removeTodo(todo.id)}>x</button>
        </label>
        
      </li>  
    {/each}
  </ul>
  <hr>
  <a href="#" on:click|preventDefault={() => filterTodos(item => item)}>All({todoItems.length})</a> |
  <a href="#" on:click|preventDefault={() => filterTodos(item => !item.checked)}>Active ({activeTodos})</a> |
  <a href="#" on:click|preventDefault={() => filterTodos(item => item.checked)}>Done ({doneTodos})</a> |
  <a href="#" on:click|preventDefault={clearCompleted}><strong>Clear completed</strong></a>
  
</main>


<style>

  .container {
    width: 500px;
    margin: 0 auto;
  }
  form {
    display: flex;
    flex-direction: column;
  }

  ul {
    list-style-type: none;
    list-style-position: outside;
    padding: 0;
  }

  li {
    display: block;
    padding: 2px;
    margin-top: 2px;
  }
  hr {
    border: 1px solid #ccc;
  }
  .remove {
    float: right;
    padding: 0 2px
  }
  .checked {  
    text-decoration: line-through;
  }
</style>