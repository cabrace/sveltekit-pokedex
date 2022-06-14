<script context="module">
    export async function load({params}){
        const id = params.id;
        const url = `https://pokeapi.co/api/v2/pokemon/${id}`;
        const res = await fetch(url);
        const pokeman = await res.json();
        return {props: {pokeman}};
    }
</script>
<script>
    export let pokeman;
    const type = pokeman.types[0].type.name;
</script>

<div class="flex flex-col items-center">

    <!-- <h1 class="text-center text-4xl my-8 uppercase">{pokeman.name} </h1> -->
    <h1 class="text-4xl text-center capitalize font-bold mt-5 mb-5">
        <span class="text-transparent bg-clip-text bg-gradient-to-r from-blue-600 via-orange-500 to-pink-600">
            { pokeman.name }
        </span>
    </h1>
    <p> Type: <strong>{type}</strong> | Height: <strong>{pokeman.height}</strong> |
    Weight: <strong>{pokeman.weight}</strong>
    
    </p>
    <img class="card-image" src={pokeman.sprites['front_default']} alt={pokeman.name} />
</div>