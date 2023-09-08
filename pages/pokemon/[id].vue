<script setup lang="ts">
import { Pokemon, Ability } from "@/interfaces/pokemon";
const { id } = useRoute().params;
const uri = "https://pokeapi.co/api/v2/pokemon/" + id;
const { data } = await useFetch<Pokemon>(uri);
const pokemon: Pokemon = data.value as Pokemon;

// Format array of abilities to capitalized, comma separated string.
const formatAbilities = (abilities: Ability[]) => {
  let abilityNames = abilities.map(({ ability }) => {
    let substr = ability.name.slice(1);
    let firstCharacterCapitalized = ability.name.charAt(0).toUpperCase();
    return firstCharacterCapitalized + substr;
  });
  return abilityNames.join(", ");
};

const formatHeight = (height: number) => {
  // Convert decimeters height to meters
  return height / 10;
};

const formatWeight = (weight: number) => {
  // Convert hectograms to kilograms
  return weight / 10;
};

const height: number = formatHeight(pokemon.height);
const weight: number = formatWeight(pokemon.weight);
const abilities: string = formatAbilities(pokemon.abilities);
const sprite: string = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${pokemon.id}.svg`;
</script>

<template>
  <div class="pokemon">
    <p class="pokemon-num">#{{ pokemon.id.toString().padStart(3, "0") }}</p>
    <img class="image" :src="`${sprite}`" />
    <h1 class="title">{{ pokemon.name }}</h1>
    <div class="details-grid">
      <div>
        <h2>Stats:</h2>
        <p class="pokemon-type">Type: {{ pokemon.types.at(0)?.type.name }}</p>
        <p>Height: {{ height }} m</p>
        <p>Weight: {{ weight }} kg</p>
      </div>
      <div>
        <h2>Abilities:</h2>
        <p class="abilities">
          {{ abilities }}
        </p>
      </div>
    </div>
  </div>
</template>

<style>
.pokemon {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-items: center;
  align-items: center;
  background-color: aquamarine;
  padding: 4rem;
  border-radius: 0.375rem;
}
.pokemon-num {
  position: absolute;
  top: 0;
  left: -1rem;
  font-size: 3rem;
  padding: 1rem;
  background-color: #212223;
  color: #f1f2f3;
  border-radius: 0.375rem;
}
.title {
  text-transform: capitalize;
}
.image {
  width: 50rem;
}
.details-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
.pokemon-type {
  text-transform: capitalize;
}
</style>
