<script setup>
/* import {ref} from "vue" */
import axios from "axios";
import { ref, onMounted } from "vue";

const img = ref();
const name = ref();
const Id = ref();
const StatHp = ref();
const Attack = ref();
const Defense = ref();
const SpecialAttack = ref();
const SpecialDefense = ref();
const Speed = ref();
const Weight = ref();
const Height = ref();
const tipoPk = ref([]);

const links1 = ref([
  "https://pokeapi.co/api/v2/pokemon/1",
  "https://pokeapi.co/api/v2/pokemon/2",
  "https://pokeapi.co/api/v2/pokemon/3",
  "https://pokeapi.co/api/v2/pokemon/4",
  "https://pokeapi.co/api/v2/pokemon/5",
  "https://pokeapi.co/api/v2/pokemon/6",
  "https://pokeapi.co/api/v2/pokemon/7",
  "https://pokeapi.co/api/v2/pokemon/8",
  "https://pokeapi.co/api/v2/pokemon/9",
  "https://pokeapi.co/api/v2/pokemon/10",
  "https://pokeapi.co/api/v2/pokemon/11",
  "https://pokeapi.co/api/v2/pokemon/12",
  "https://pokeapi.co/api/v2/pokemon/13",
  "https://pokeapi.co/api/v2/pokemon/14",
  "https://pokeapi.co/api/v2/pokemon/15",
  "https://pokeapi.co/api/v2/pokemon/16",
  "https://pokeapi.co/api/v2/pokemon/17",
  "https://pokeapi.co/api/v2/pokemon/18",
  "https://pokeapi.co/api/v2/pokemon/19",
  "https://pokeapi.co/api/v2/pokemon/20",
  "https://pokeapi.co/api/v2/pokemon/21",
  "https://pokeapi.co/api/v2/pokemon/22",
  "https://pokeapi.co/api/v2/pokemon/23",
  "https://pokeapi.co/api/v2/pokemon/24",
  "https://pokeapi.co/api/v2/pokemon/25",
  "https://pokeapi.co/api/v2/pokemon/26",
  "https://pokeapi.co/api/v2/pokemon/27",
  "https://pokeapi.co/api/v2/pokemon/28",
  "https://pokeapi.co/api/v2/pokemon/29",
  "https://pokeapi.co/api/v2/pokemon/30",
  "https://pokeapi.co/api/v2/pokemon/31",
  "https://pokeapi.co/api/v2/pokemon/32",
  "https://pokeapi.co/api/v2/pokemon/33",
  "https://pokeapi.co/api/v2/pokemon/34",
  "https://pokeapi.co/api/v2/pokemon/35",
  "https://pokeapi.co/api/v2/pokemon/36",
  "https://pokeapi.co/api/v2/pokemon/37",
  "https://pokeapi.co/api/v2/pokemon/38",
  "https://pokeapi.co/api/v2/pokemon/39",
  "https://pokeapi.co/api/v2/pokemon/40",
  "https://pokeapi.co/api/v2/pokemon/41",
  "https://pokeapi.co/api/v2/pokemon/42",
  "https://pokeapi.co/api/v2/pokemon/43",
  "https://pokeapi.co/api/v2/pokemon/44",
  "https://pokeapi.co/api/v2/pokemon/45",
  "https://pokeapi.co/api/v2/pokemon/46",
  "https://pokeapi.co/api/v2/pokemon/47",
  "https://pokeapi.co/api/v2/pokemon/48",
  "https://pokeapi.co/api/v2/pokemon/49",
  "https://pokeapi.co/api/v2/pokemon/50",
]);
const pokemonData = ref([]);
const Mostrar = ref(false);
async function obtenerurlsPokemon() {
  pokemonData.value = [];
  for (const link of links1.value) {
    let data = await axios.get(link);
    pokemonData.value.push({
      img: data.data.sprites.other["official-artwork"].front_default,
      name: data.data.name,
      Id: data.data.id,
      StatHp: data.data.stats[0].base_stat,
      Attack: data.data.stats[1].base_stat,
      Defense: data.data.stats[2].base_stat,
      SpecialAttack: data.data.stats[3].base_stat,
      SpecialDefense: data.data.stats[4].base_stat,
      Speed: data.data.stats[5].base_stat,
      Weight: data.data.weight,
      Height: data.data.height,
      tipoPk: data.data.types.map((element) => element.type.name),
    });
  }
  /* Mostrar.value = true */
}
async function obtenerDetallesPokemon() {
  pokemonData.value = [];
  for (const link of links1.value) {
    let data = await axios.get(link);
    pokemonData.value.push({
      img: data.data.sprites.other["official-artwork"].front_default,
      name: data.data.name,
      Id: data.data.id,
      StatHp: data.data.stats[0].base_stat,
      Attack: data.data.stats[1].base_stat,
      Defense: data.data.stats[2].base_stat,
      SpecialAttack: data.data.stats[3].base_stat,
      SpecialDefense: data.data.stats[4].base_stat,
      Speed: data.data.stats[5].base_stat,
      Weight: data.data.weight,
      Height: data.data.height,
      tipoPk: data.data.types.map((element) => element.type.name),
    });
  }
  Mostrar.value = true
}
function getColorByType(tipo) {
  if (tipo === "fire") {
    return "#FDDFDF";
  } else if (tipo === "grass") {
    return "#DEFDE0";
  } else if (tipo === "electric") {
    return "#FCF7DE";
  } else if (tipo === "water") {
    return "#DEF3FD";
  } else if (tipo === "ground") {
    return "#f4e7da";
  } else if (tipo === "rock") {
    return "#d5d5d4";
  } else if (tipo === "fairy") {
    return "#fceaff";
  } else if (tipo === "poison") {
    return "#98d7a5";
  } else if (tipo === "bug") {
    return "#f8d5a3";
  } else if (tipo === "dragon") {
    return "#97b3e6";
  } else if (tipo === "psychic") {
    return "#eaeda1";
  } else if (tipo === "flying") {
    return "#F5F5F5";
  } else if (tipo === "fighting") {
    return "#E6E0D4";
  } else {
    return "white"; // Si el tipo no se encuentra, establece el color en blanco
  }
} // Si el tipo no se encuentra, se establece en blanco





onMounted(() => {
  obtenerurlsPokemon(); // Llama a la función cuando la página se carga
});
</script>

<template>
  <div class="body">
    <div class="nav">
      <h1>Pokedex</h1>
    </div>

    <div class="main">
      <buuton v-for="(pokemon, index) in pokemonData" :key="index"
      :style="{ backgroundColor: getColorByType(pokemon.tipoPk[0]) }"
      class="Tarjeta"
      @click="obtenerDetallesPokemon()">
        <div class="containerImgTarjeta">
          <img :src="pokemon.img" alt="" class="imgTarjeta" />
        </div>
        <div class="containerDatosTarjeta">
          <p style="margin: 0;" class="IdTarjeta">#{{ pokemon.Id }}</p>
          <p style="margin: 0;">{{ pokemon.name }}</p>
          <p style="margin: 0;">Tipo: {{ pokemon.tipoPk[0] }} {{ pokemon.tipoPk[1] }} </p>
        </div>
      </buuton>
    </div>

    <div class="Detalles" v-if="Mostrar">
      <div class="funcion" v-if="Mostrar">
        <div class="funcionPart1">
          <div class="nameContainer">
            <h2 class="name">{{ name }}</h2>
          </div>

          <div class="containerImg">
            <h2 class="Id">#{{ Id }}</h2>
            <img :src="img" class="img" />
          </div>
          <div class="Types">
            <h2 v-for="(item, index) in tipoPk" :key="index" class="types">
              {{ item }}
            </h2>
          </div>
        </div>
        <div>
          <div class="datos" v-for="(i, index) in pokemonData" :key="index">
            <h2>HP: {{ i.StatHp }}</h2>
            <h2>Attack: {{ i.Attack }}</h2>
            <h2>Defense: {{ i.Defense }}</h2>
            <h2>SpecialAttack: {{ i.SpecialAttack }}</h2>
            <h2>SpecialDefense: {{ i.SpecialDefense }}</h2>
            <h2>Speed: {{ i.Speed }}</h2>
            <h2>Weight: {{ i.Weight }}KG</h2>
            <h2>Height: {{ i.Height }}</h2>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: black;
  color: white;
  margin: 0;
  gap: 15px;
  font-family: "Pa ver";
}

@font-face {
  font-family: "Pa ver";
  src: url("./fonts/Anta-Regular.ttf");
}

.img {
  width: 500px;
}

.imgTarjeta {
  width: 100px;
}
.Tarjeta {
  display: flex;
  flex-direction: column;
  align-items: center;
 color: black;
  border-radius: 15px;
  padding: 15px;
  gap: 15px;
  cursor: pointer;
}
.containerDatosTarjeta{
  display: flex;
  flex-direction: column;
  align-items: center;
  
}
.containerImgTarjeta{
  padding: 15px;
  background-color: white;
  border-radius: 50%;
}
.botonPeticion {
  border-radius: 10px;
  padding: 15px;
  border: none;
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
  font-size: 20px;
  cursor: pointer;
  letter-spacing: 1.5px;
}

.botonPeticion:hover {
  background-color: rgb(0, 89, 255);
  transition: 0.5s ease-in-out;
  color: white;
}

.nav {
  background-color: rgb(0, 89, 255);
  margin: 0;
  display: flex;
  justify-content: center;
}

.main {
  display: flex;
  justify-content: center;
  padding: 0px 100px;
  flex-wrap: wrap;
  gap: 85px;
}

.Detalles {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.funcion {
  display: flex;
  gap: 65px;
  flex-wrap: wrap;
  align-items: center;
}

.datos {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}

.containerImg {
  display: flex;
  align-items: center;
  padding: 0;
}

.img {
  width: 400px;
}

.Types {
  display: flex;
  width: 100%;
  justify-content: space-around;
}

.types {
  background-color: rgb(0, 89, 255);
  padding: 10px;
  border-radius: 10px;
}

.Id {
  font-size: 150px;
  color: rgb(0, 89, 255);
  margin: 0;
}
.IdTarjeta{
  padding: 5px;
  background-color: rgba(0, 0, 0, 0.337);
  border-radius: 5px;
}
.name {
  font-size: 100px;
  margin: 0;
}
.funcionPart1 {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
