<script>
    import TodoItem from './TodoItem.svelte';
    import ListItem from './ListItem.svelte';

    let newTodoTitle = '';
    let currentFilter = 'all';
    let nextId = 4;
    let todos = [
        {
            id: 1,
            title: 'Terminar trabajos practicos',
            completed: false
        },
        {
            id: 2,
            title: 'Sacar turno con el doctor',
            completed: false
        },
        {
            id: 3,
            title: 'Salir a correr',
            completed: false
        }
    ];
    function addTodo(event) {
        if (event.key === 'Enter') {
            todos = [...todos, {
                id: nextId,
                completed: false,
                title: newTodoTitle
            }];
            nextId = nextId + 1;
            newTodoTitle = '';
        }
    }
    $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
    $: filteredTodos = currentFilter === 'all' ? todos : currentFilter === 'completed'
        ? todos.filter(todo => todo.completed)
        : todos.filter(todo => !todo.completed)
    function checkAllTodos(event) {
        todos.forEach(todo => todo.completed = event.target.checked);
        todos = todos;
    }
    function updateFilter(newFilter) {
        currentFilter = newFilter;
    }
    function clearCompleted() {
        todos = todos.filter(todo => !todo.completed);
    }
    function handleDeleteTodo(event) {
        todos = todos.filter(todo => todo.id !== event.detail.id);
    }
    function handleToggleComplete(event) {
        const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
        const updatedTodo = { ...todos[todoIndex], completed: !todos[todoIndex].completed};
        todos = [
            ...todos.slice(0, todoIndex),
            updatedTodo,
            ...todos.slice(todoIndex+1)
        ];
    }
</script>

<style>
    .container {
        max-width: 800px;
        margin: 10px auto;
    }
    .todo-input {
        width: 100%;
        padding: 10px, 20px;
        font-size: 18px;
        margin-bottom: 20px;
    }
    .inner-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 15px;
        margin-bottom: 13px;
    }
    .inner-container-input {
        margin-right: 12px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
    }
    button:hover {
        background: lightseagreen;
    }
    button:focus {
        outline: none;
    }
    .active {
        background: lightseagreen;
    }
</style>

<div class="container">

    <h2>Lista de cosas por hacer:</h2>
    <input type="text" class="todo-input" placeholder="Agregar una tarea nueva ..." bind:value={newTodoTitle} on:keydown={addTodo} >

    {#each filteredTodos as todo}
        <div class="todo-item">
            <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
        </div>
    {/each}

    <div class="inner-container">
        <div><label><input class="inner-container-input" type="checkbox" on:change={checkAllTodos}>Seleccionar todos</label></div>
        <div>{todosRemaining} tareas por completar</div>
    </div>

    <div class="inner-container">
        <div>
            <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">Todos</button>
            <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Activos</button>
            <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completados</button>
        </div>
        <dir>
            <button on:click={clearCompleted}>Eliminar tareas finalizadas</button>
        </dir>
    </div>
    
    <div class="list-item">
        <ListItem />
    </div>

</div>