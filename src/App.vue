<template>
  <div class="body">
    <div class="nav">
      <button class="PokeRes" @click="Volver()">Pokedex</button>
    </div>

    <div class="main" v-if="MostrarMain">
      <button
        v-for="(pokemon, index) in pokemonData"
        :key="index"
        :style="{ backgroundColor: getColorByType(pokemon.tipo_pk[0]) }"
        @click="obtenerDetallesPokemon(pokemon)"
        class="Tarjeta"
      >
        <div class="containerImgTarjeta">
          <img :src="pokemon.img" alt="" class="imgTarjeta" />
        </div>
        <div class="containerDatosTarjeta">
          <p style="margin: 0" class="IdTarjeta">#{{ pokemon.numero }}</p>
          <p style="margin: 0" class="NameTarjeta">{{ pokemon.nombre }}</p>
          <span style="display: flex"
            ><p
              style="margin: 0px 7px"
              class="Typetarjeta"
              v-for="(tipo, index) in pokemon.tipo_pk"
              :key="index"
              :style="{ backgroundColor: getColorByTypeDetalle(tipo) }"
            >
              {{ tipo }}
            </p>
            <!-- <p
              style="margin: 0px 7px"
              class="Typetarjeta"
              v-for="(tipo, index) in pokemon.tipo_pk"
              :key="index"
              :style="{ backgroundColor: getColorByTypeDetalle(tipo) }"
            >
              {{ tipo}}
            </p> --></span
          >
        </div>
      </button>
    </div>

    <div
      class="Detalles"
      v-if="Mostrar"
      :style="{ background: getColorByTypeFondo(tipoPk[0]) }"
    >
      <div class="funcion">
        <div class="funcionPart1">
          <div class="nameContainer">
            <h2 class="name">{{ name }}</h2>
          </div>

          <div class="containerImg">
            <h2 class="Id">#{{ Id }}</h2>
            <img :src="img" class="img" />
          </div>
          <div class="Types">
            <h2
              v-for="(item, index) in tipoPk"
              :key="index"
              class="types"
              :style="{ backgroundColor: getColorByTypeDetalle(item) }"
            >
              {{ item }}
            </h2>
          </div>
        </div>
        <div>
          <div class="datos">
            <h2>{{ nombre }}</h2>

            <h2>HP: {{ StatHp }}</h2>
            <h2>Attack: {{ Attack }}</h2>
            <h2>Defense: {{ Defense }}</h2>
            <h2>SpecialAttack: {{ SpecialAttack }}</h2>
            <h2>SpecialDefense: {{ SpecialDefense }}</h2>
            <h2>Speed: {{ Speed }}</h2>
            <h2>Weight: {{ Weight }}KG</h2>
            <h2>Height: {{ Height }}</h2>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";

let img = ref("");
let name = ref("");
let Id = ref("");
let StatHp = ref("");
let Attack = ref("");
let Defense = ref("");
let SpecialAttack = ref("");
let SpecialDefense = ref("");
let Speed = ref("");
let Weight = ref("");
let Height = ref("");
let tipoPk = ref([]);

let pokemonData = ref([]);
let Mostrar = ref(false);
let MostrarMain = ref(true);

async function obtenerDetallesPokemon(pokemon) {
  try {
    const response = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/${pokemon.numero}`
    );
    const data = response.data;

    img.value = data.sprites.other["official-artwork"].front_default;
    name.value = data.name;
    Id.value = data.id;
    StatHp.value = data.stats[0].base_stat;
    Attack.value = data.stats[1].base_stat;
    Defense.value = data.stats[2].base_stat;
    SpecialAttack.value = data.stats[3].base_stat;
    SpecialDefense.value = data.stats[4].base_stat;
    Speed.value = data.stats[5].base_stat;
    Weight.value = data.weight;
    Height.value = data.height;
    tipoPk.value = data.types.map((element) => element.type.name);

    Mostrar.value = true;
    MostrarMain.value = false;
  } catch (error) {
    console.error("Error al obtener detalles del Pokémon:", error);
  }
}

function Volver() {
  img.value = "";
  Id.value = "";
  name.value = "";
  StatHp.value = "";
  Attack.value = "";
  Defense.value = "";
  SpecialAttack.value = "";
  SpecialDefense.value = "";
  Speed.value = "";
  tipoPk.value = [];
  Mostrar.value = false;
  MostrarMain.value = true;
}

function getColorByType(tipo) {
  switch (tipo) {
    case "fire":
      return "#FDDFDF";
    case "grass":
      return "#DEFDE0";
    case "electric":
      return "#FCF7DE";
    case "water":
      return "#DEF3FD";
    case "ground":
      return "#f4e7da";
    case "rock":
      return "#d5d5d4";
    case "fairy":
      return "#fceaff";
    case "poison":
      return "#E1D1FF";
    case "bug":
      return "#E6FFC9";
    case "dragon":
      return "#97b3e6";
    case "psychic":
      return "#eaeda1";
    case "flying":
      return "#F5F5F5";
    case "fighting":
      return "#E6E0D4";
    default:
      return "white";
  }
}

function getColorByTypeDetalle(tipo) {
  switch (tipo) {
    case "fire":
      return "#FF2D00";
    case "grass":
      return "#009121";
    case "electric":
      return "#8D8F00";
    case "water":
      return "#00B7FF";
    case "ground":
      return "#635200";
    case "rock":
      return "#525252";
    case "fairy":
      return "#ED7FFF";
    case "poison":
      return "#BC00FF";
    case "bug":
      return "#87C800";
    case "dragon":
      return "#AC0000";
    case "psychic":
      return "#008F88";
    case "flying":
      return "#6D6984";
    case "fighting":
      return "#663030";
    default:
      return "3A6630";
  }
}

function getColorByTypeFondo(tipo) {
  switch (tipo) {
    /*  case "fire":
      return "#FF2D00";
    case "grass":
      return "#009121"; */
    case "electric":
    return "url('./assets/Electric.jpg')";
    /* case "water":
      return "#00B7FF";
    case "ground":
      return "#635200";
    case "rock":
      return "#525252";
    case "fairy":
      return "#ED7FFF"; */
    case "poison":
      return "url('./assets/Poison.jpg')";
    /* case "bug":
      return "#87C800";
    case "dragon":
      return "#AC0000";
    case "psychic":
      return "#008F88";
    case "flying":
      return "#6D6984";
    case "fighting":
      return "#663030"; */
    default:
      return "white";
  }
}

onMounted(() => {
  obtenerUrlsPokemon(); // Llama a la función cuando la página se carga
});

async function obtenerUrlsPokemon() {
  try {
    for (let i = 1; i <= 50; i++) {
      const response = await axios.get(
        `https://pokeapi.co/api/v2/pokemon/${i}`
      );
      const data = response.data;

      pokemonData.value.push({
        img: data.sprites.other["official-artwork"].front_default,
        numero: data.id,
        nombre: data.name,
        hp: data.stats[0].base_stat,
        attack: data.stats[1].base_stat,
        defense: data.stats[2].base_stat,
        specialattack: data.stats[3].base_stat,
        specialdefense: data.stats[4].base_stat,
        speed: data.stats[5].base_stat,
        tipo_pk: data.types.map((element) => element.type.name),
      });
    }
  } catch (error) {
    console.error("Error al obtener la lista de Pokémon:", error);
  }
}
</script>
<style scoped>
.body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: white;
  margin: 0;
  font-family: "Pa ver";
  padding: 100px 0px 0px 0px;
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
  width: 200px;
  border: none;
}

.containerDatosTarjeta {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 25px;
  gap: 10px;
  font-weight: 500;
}

.containerImgTarjeta {
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
  padding: 5px 0px;
  position: fixed;
  top: 0;
  width: 100%;
}

.main {
  display: flex;
  justify-content: center;
  padding: 0px 200px;
  flex-wrap: wrap;
  gap: 85px;
}

.Detalles {
  display: flex;
  justify-content: center;
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
.PokeRes {
  background-color: transparent;
  border: none;
  font-family: "Pa ver";
  color: white;
  font-size: 55px;
  cursor: pointer;
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
  padding: 10px;
  border-radius: 10px;
}

.Id {
  font-size: 150px;
  color: rgb(0, 89, 255);
  margin: 0;
}

.IdTarjeta {
  padding: 5px;
  background-color: rgba(0, 0, 0, 0.337);
  border-radius: 5px;
}
.NameTarjeta::first-letter,
.Typetarjeta::first-letter {
  text-transform: uppercase;
}

.Typetarjeta {
  padding: 5px;
  border-radius: 15px;
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
