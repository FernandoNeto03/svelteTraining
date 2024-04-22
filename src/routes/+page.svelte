<svelte:head>
  <link
    rel="stylesheet"
    href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css"
  />
</svelte:head>

<script>
    import { onMount } from 'svelte';

    let toDoList = [];
    let textInput = "";
    let nextPokemonId = 1;

    onMount(() => {
        if (toDoList.length === 0) {
            toDoList.push({content: "Fazer o trabalho do Guto!", editing: false, checked: true, pokemonGif: null});
            fetchPokemonGif(nextPokemonId++, toDoList.length - 1);
        }
    });

    function addToDo() {
        if (textInput.trim() !== "") {
            toDoList = [{content: textInput, editing: false, checked: false, pokemonGif: null}, ...toDoList];
            fetchPokemonGif(nextPokemonId++, 0);
            textInput = "";
        }
    }

    function fetchPokemonGif(pokemonId, index) {
        let gifUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${pokemonId}.gif`;
        toDoList[index].pokemonGif = gifUrl;
        toDoList = toDoList;
    }

    function setEditing(i, isEditing) {
        toDoList[i].editing = isEditing;
    }

    function deleteTodo(i) {
        toDoList.splice(i, 1);
        toDoList = toDoList;
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

{#each toDoList as toDo, i}
<div style="display: flex; align-items: center; width: 700px; margin: 0 auto;">
    {#if toDo.editing}
        <input type="text" bind:value={toDo.content}>
    {:else}
        <input type="checkbox" bind:checked={toDo.checked}>
        <img src={toDo.pokemonGif} alt={`GIF animado do Pokémon número ${i+1}`} style="width: 50px; height: 50px; margin-right: 10px;">
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
