<svelte:head>
  <link
    rel="stylesheet"
    href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css"
  />
</svelte:head>

<script>
    let toDoList = [{content: "Fazer o trabalho do Guto!", editing: false, checked:true}];
    let textInput = "";

    function addToDo() {
        toDoList = [...toDoList, {content:textInput , editing: false, checked:false}]
    }

    function setEditing(i, isEditing) {
        toDoList[i].editing= isEditing; //true or false
    }

    function deleteTodo(i) {
        toDoList.splice(i, 1);
        toDoList = toDoList; //https://svelte.dev/tutorial/updating-arrays-and-objects
        // Estranho mas tem que 'dizer pro Svelte' que estamos dando update
        //no valor da lista de toDo
    }
</script>

<div style="margin: 0 auto; padding: 20px; width: 700px">
  <h2 style="text-align: center;">Todo List</h2>
  <p>Insira suas tarefas aqui!</p>
  <div style="display: flex; justify-content: center">
    <input type="text" style="margin: 10px" bind:value={textInput}/>
    <button style="width: 300px; height:80px" on:click={addToDo}>Add</button>
  </div>
</div>

{#each toDoList as toDo, i }
<div style="display: flex; align-items: baseline; width: 700px; margin:0 auto;">
    {#if toDo.editing}
        <input type="text" bind:value={toDo.content}>
    {:else}
    <input type="checkbox" bind:checked={toDo.checked}>
    <h4 style="flex-grow: 1">{toDo.content}</h4>
    {/if}
    <div style="display: flex;">
        {#if toDo.editing}
            <button on:click={() => setEditing(i, false)}>Save</button>
        {:else}
            <button on:click={() => setEditing(i, true)}>Edit</button>
        {/if}
        <button on:click={() => deleteTodo(i)}>Delete</button>
    </div>
</div>
{/each}