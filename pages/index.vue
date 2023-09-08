<script lang="ts" setup>
import { PokemonAll } from "~/interfaces/pokemon";
const currentPokemon = ref([
  {
    name: "",
    id: "",
    url: "",
    image: "",
  },
]);
const searchTerm = ref("");

async function fetchPokemon() {
  const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=60");
  const data: PokemonAll = await res.json();

  currentPokemon.value = data.results.map((pokemon) => {
    return {
      name: pokemon.name,
      id: pokemon.url.split("/")[6],
      url: pokemon.url,
      image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${
        pokemon.url.split("/")[6]
      }.svg`,
    };
  });
}

fetchPokemon();

function searchPokemon() {
  let filteredPokemon = currentPokemon.value;
  if (currentPokemon.value.length > 0 && searchTerm.value) {
    filteredPokemon = currentPokemon.value.filter((pokemon) => {
      return pokemon.name.startsWith(searchTerm.value);
    });
  } else if (searchTerm.value === "") {
    fetchPokemon();
  }
  currentPokemon.value = filteredPokemon;
}
</script>

<template>
  <div>
    <div class="search-container">
      <img
        class="logo"
        src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1280px-International_Pok%C3%A9mon_logo.svg.png"
        alt="Pokémon logo"
      />
      <input
        class="search-input"
        type="text"
        v-model="searchTerm"
        placeholder="Search..."
        @input="searchPokemon()"
      />
    </div>
    <ul class="card-grid">
      <li class="card" v-for="p in currentPokemon" :key="p.id">
        <p class="card-num">#{{ p.id.toString().padStart(3, "0") }}</p>
        <NuxtLink class="card-content" :to="`/pokemon/${p.id}`">
          <p class="card-title">{{ p.name }}</p>
          <img class="card-image" :src="`${p.image}`" />
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<style>
.card {
  position: relative;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);
  background-color: aquamarine;
  border-radius: 0.375rem;
}
.card:hover {
  transform: scale(110%);
  transition-property: transform;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 150ms;
}
.card-content {
  text-decoration: none;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-items: center;
  align-items: center;
}
.card-grid {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}
.card-image {
  max-height: 10rem;
  max-width: 10rem;
  padding: 1rem;
}
.card-title {
  text-transform: capitalize;
  font-size: 2rem;
  color: #212223;
}
.logo {
  width: 30rem;
  place-self: center;
}
.search-container {
  display: grid;
}
.search-input {
  max-width: 30rem;
  margin-top: 1.75rem;
  padding: 1rem 1.5rem;
  border: none;
  border: solid 1px #ccc;
}
.card-num {
  position: absolute;
  top: -1rem;
  left: -0.75rem;
  font-size: 1.75rem;
  padding: 0.75rem;
  background-color: #212223;
  color: #f1f2f3;
  border-radius: 0.375rem;
}
</style>
