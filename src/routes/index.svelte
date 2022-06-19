<script context="module">
  export async function load({params}){
    const url = `https://pokeapi.co/api/v2/pokemon?limit=40`;
    const res = await fetch(url);
    const data = await res.json();

    const loadedPokemon = data.results.map( (data, index) => {

        return {
            name: data.name,
            id: index + 1,
            image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`
        }
    });
    return {props: {pokemon:loadedPokemon}}
  }
</script>
<script>
  /* INstead of importing pokemon from the Store we use export let pokemon. */
  /* import { pokemon } from "../stores/pokestore"; */

  import PokemanCard from "../components/pokemanCard.svelte";
  import { validate_each_argument } from "svelte/internal";

  export let pokemon;

  // let nameSearchTerm = "";
  // let idSearchTerm = "";
  let searchTerm = "";
  let filteredPokemon = [];
  const regex = /\d* \w*/i


  $: {
   console.log(searchTerm);

    if (searchTerm){

      filteredPokemon = pokemon.filter(
          pokeman => (  
          pokeman.name.toLowerCase().includes(searchTerm.toLowerCase()) || 
          pokeman.id.toString().includes(searchTerm)
          )
        );
    }
    else {
        filteredPokemon = [...pokemon];
    }

  }
</script>

<svelte:head>
  <title>Svelte Kit Pokedex</title>
</svelte:head>

<div class="container mx-auto">
<h1 class="text-4xl text-center font-bold mt-5">
    <span class="text-transparent bg-clip-text bg-gradient-to-r from-blue-600 via-orange-500 to-pink-600">
         Pokedex
    </span>
</h1>

<div class="flex justify-between gap-4 mt-3 mb-1">
  <input class="w-full rounded-lg text-lg p-4 border-2 border-gray-300" bind:value={searchTerm} placeholder="Search Pokemon by Name">
</div>

<div class="py-4 grid gap-4 md:grid-cols-2 grid-cols-1">
  {#each filteredPokemon as pokeman}
    <PokemanCard pokeman={pokeman} />
  {/each}
</div>

</div>


