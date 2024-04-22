<svelte:head>
  <link
    rel="stylesheet"
    href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css"
    />
</svelte:head>

<script>
    import { onMount } from 'svelte';
    import { flip } from 'svelte/animate';
    import { dndzone } from 'svelte-dnd-action';

    let toDoList = [];
    let textInput = "";
    let nextPokemonId = 1;

    function handleConsider(event) {
        toDoList = event.detail.items;
    }

    function handleFinalize(event) {
        toDoList = event.detail.items;
    }

    onMount(() => {
        if (toDoList.length === 0) {
            toDoList.push({
                id: nextPokemonId,
                content: "Fazer o trabalho do Guto!",
                editing: false,
                checked: true,
                pokemonGif: null
            });
            fetchPokemonGif(nextPokemonId++, toDoList.length - 1);
        }
    });

    function addToDo() {
        if (textInput.trim() !== "") {
            toDoList = [{ id: nextPokemonId, content: textInput, editing: false, checked: false, pokemonGif: null }, ...toDoList];
            fetchPokemonGif(nextPokemonId++, 0);
            textInput = "";
        }
    }

    function fetchPokemonGif(pokemonId, index) {
        let gifUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${pokemonId}.gif`;
        toDoList[index].pokemonGif = gifUrl;
        toDoList = [...toDoList];
    }

    function setEditing(i, isEditing) {
        toDoList[i].editing = isEditing;
    }

    function deleteTodo(i) {
        toDoList.splice(i, 1);
        toDoList = [...toDoList];
    }
</script>

<div class="todo-container">
    <h2>Todo List</h2>
    <p>Insira suas tarefas aqui!</p>
    <div id="todo-container-div">
        <input type="text" id="todo-container-div-input" bind:value={textInput}/>
        <button id="todo-container-div-button" on:click={addToDo}>Add</button>
    </div>
</div>

<section use:dndzone={{items: toDoList, flipDurationMs: 100}} on:consider={handleConsider} on:finalize={handleFinalize}>
    {#each toDoList as toDo, i (toDo.id)}
    <div class="todo-item" animate:flip>
        {#if toDo.editing}
            <input type="text" bind:value={toDo.content}>
        {:else}
            <input type="checkbox" bind:checked={toDo.checked}>
            <img src={toDo.pokemonGif} alt={`Pokemon GIF number ${i+1}`} id="pokemon-img">
            <h4>{toDo.content}</h4>
        {/if}
        <div class="todo-buttons">
            {#if toDo.editing}
                <button on:click={() => setEditing(i, false)}>Save</button>
            {:else}
                <button on:click={() => setEditing(i, true)}>Edit</button>
            {/if}
            <button on:click={() => deleteTodo(i)}>Delete</button>
        </div>
    </div>
    {/each}
</section>

<style>
    .todo-container {
        margin: 0 auto;
        padding: 20px;
        max-width: 700px;
        width: 100%;
    }

    .todo-item {
        display: flex;
        align-items: center;
        margin: 5px auto;
        padding: 10px;
        border-radius: 10px;
        background-color: #dadada;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        max-width: 800px;
    }

    .todo-buttons button {
        margin-right: 5px;
    }

    div .todo-buttons{
        display: flex;
    }

    #todo-container-div{
        display: flex; 
        justify-content: center;
    }

    h2{
        text-align: center;
    }

    h4{
        flex-grow: 1;
        color:black;
    }

    #todo-container-div-input{
        margin: 10px;
    }

    #pokemon-img{
        width: 50px; 
        height: 50px; 
        margin-right: 10px;
    }

    #todo-container-div-button{
        height:80px;
    }

    @media (max-width: 600px) {
        .todo-container {
            padding: 10px;
        }

        .todo-item {
            flex-direction: column;
        }

        .todo-buttons button {
            margin-bottom: 5px;
        }
    }
</style>