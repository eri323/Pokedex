<template>
  <div class="body">
    <div class="nav">
      <button class="PokeRes" @click="Volver()">Pokedex</button>
      <div class="ContainerOpcs" v-if="MostrarMain">
        <div class="input-wrapper">
          <button class="icon">
            Buscar
            <i class="fa-solid fa-magnifying-glass" style="color: black"></i>
          </button>
          <input v-model="searchQuery" class="input" name="text" type="text" />
        </div>
        <button class="BtnFiltrar" @click="MostrarFiltro()">
          <i class="fa-sharp fa-solid fa-filter" style="color: #000000"></i>
          Filtrar
        </button>
        <div class="ContainerFiltrar" v-if="MostrarFiltrar">
          <select v-model="selectedType" class="Select">
            <option value="">Todos</option>
            <option v-for="type in availableTypes" :key="type" :value="type">
              {{ type }}
            </option>
          </select>
        </div>
      </div>
    </div>

    <div class="main" v-if="MostrarMain">
      <div class="ContainerTarjetas">
        <button
          v-for="(pokemon, index) in filteredPokemonData"
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
            <span style="display: flex">
              <p
                style="margin: 0px 7px"
                class="Typetarjeta"
                v-for="(tipo, index) in pokemon.tipo_pk"
                :key="index"
                :style="{ backgroundColor: getColorByTypeDetalle(tipo) }"
              >
                {{ tipo }}
              </p>
            </span>
          </div>
        </button>
      </div>
      <div class="ContainerBtnVerMas">
        <button
          class="BtnVerMas"
          @click="obtenerUrlsPokemon2()"
          :disabled="currentPage * 51 > 1200"
        >
          <div>
            <span>
              <p>Ver Mas</p>
            </span>
          </div>
          <div>
            <span>
              <i class="fa-solid fa-angles-down fa-beat fa-2xl" id="Plus"></i>
            </span>
          </div>
        </button>
      </div>
    </div>

    <div
      class="Detalles"
      v-if="Mostrar"
      :style="{ backgroundImage: backgroundImage }"
    >
      <div class="NamePokemon">
        <div class="nameContainer">
          <h2 class="name">{{ name }}</h2>
        </div>
      </div>
      <div class="funcion">
        <div class="funcionPart1">
          <div class="containerImg">
            <h2 class="Id">#{{ Id }}</h2>
            <img :src="img" class="img" />
          </div>
          <div class="ConatinerPesoAltura">
            <div class="ContainerPeso">
              <h2>Weight</h2>
              <h2>{{ Weight }} Kg</h2>
            </div>
            <div class="ContainerAltura">
              <h2>Height</h2>
              <h2>{{ Height }} M</h2>
            </div>
          </div>
        </div>
        <div class="containerDatos">
          <div class="datos">
            <h2>
              HP
              <div class="progress-bar">
                <div
                  class="progress"
                  :style="{ width: (StatHp / 250) * 100 + '%' }"
                ></div>
              </div>
              {{ StatHp }}
            </h2>

            <h2>
              Attack
              <div class="progress-bar">
                <div
                  class="progress"
                  :style="{ width: (Attack / 250) * 100 + '%' }"
                ></div>
              </div>
              {{ Attack }}
            </h2>
            <h2>
              Defense
              <div class="progress-bar">
                <div
                  class="progress"
                  :style="{ width: (Defense / 250) * 100 + '%' }"
                ></div>
              </div>
              {{ Defense }}
            </h2>
            <h2>
              SpecialAttack
              <div class="progress-bar">
                <div
                  class="progress"
                  :style="{ width: (SpecialAttack / 250) * 100 + '%' }"
                ></div>
              </div>
              {{ SpecialAttack }}
            </h2>
            <h2>
              SpecialDefense
              <div class="progress-bar">
                <div
                  class="progress"
                  :style="{ width: (SpecialDefense / 250) * 100 + '%' }"
                ></div>
              </div>
              {{ SpecialDefense }}
            </h2>
            <h2>
              Speed
              <div class="progress-bar">
                <div
                  class="progress"
                  :style="{ width: (Speed / 250) * 100 + '%' }"
                ></div>
              </div>
              {{ Speed }}
            </h2>
          </div>
        </div>
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
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted, computed } from "vue";
/* import electric from './assets/electric.jpg';
import posion from './assets/posion.jpg'; */
/* const pokemonImages = ref({
  electric,
  posion
})
 */

let img = ref("");
let name = ref("");
let Id = ref("");
let StatHp = ref("");
let Attack = ref("");
let Defense = ref("");
let SpecialAttack = ref("");
let SpecialDefense = ref("");
let searchQuery = ref("");
let Speed = ref("");
let Weight = ref("");
let Height = ref("");
let tipoPk = ref([]);
let loading = ref(false);
let currentPage = ref(1);
let selectedTypes = ref({
  fire: false,
  grass: false,
  electric: false,
  water: false,
  ground: false,
  rock: false,
  fairy: false,
  posion: false,
  bug: false,
  dragon: false,
  psychic: false,
  flying: false,
  fighting: false,
});

let pokemonData = ref([]);
let Mostrar = ref(false);
let MostrarMain = ref(true);
let MostrarFiltrar = ref(false);
function search() {
  const query = searchQuery.value.toLowerCase();
  return pokemonData.value.filter((pokemon) =>
    pokemon.nombre.toLowerCase().includes(query)
  );
}

const availableTypes = [
  "fire",
  "grass",
  "electric",
  "water",
  "ground",
  "rock",
  "fairy",
  "poison",
  "bug",
  "dragon",
  "psychic",
  "flying",
  "fighting",
  "normal",
  "ice",
  "dark",
  "ghost",
];

let selectedType = ref("");

const filteredPokemonData = computed(() => {
  let filteredData = pokemonData.value;

  if (selectedType.value) {
    filteredData = filteredData.filter((pokemon) =>
      pokemon.tipo_pk.includes(selectedType.value)
    );
  }

  if (searchQuery.value.trim() !== "") {
    const query = searchQuery.value.toLowerCase();
    filteredData = filteredData.filter((pokemon) =>
      pokemon.nombre.toLowerCase().includes(query)
    );
  }

  return filteredData;
});

let backgroundImage = ref("");
function filtrarPokemon() {
  filteredPokemonData.value = pokemonData.value.filter((pokemon) => {
    return tipoPk.value.some((tipo) => selectedTypes.value[tipo]);
  });
}

async function obtenerDetallesPokemon(pokemon) {
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
  Weight.value = data.weight / 10;
  Height.value = data.height / 10;
  tipoPk.value = data.types.map((element) => element.type.name);

  backgroundImage.value = `url('./src/assets/${tipoPk.value[0]}.jpg')`;

  Mostrar.value = true;
  MostrarMain.value = false;
  MostrarFiltrar.value = false;
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
  searchQuery.value = "";
  Mostrar.value = false;
  MostrarMain.value = true;
  MostrarFiltrar.value = false;
}
function MostrarFiltro() {
  MostrarFiltrar.value = true;
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
    case "ghost":
      return "#c9c9c9";
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
    case "dark":
      return "#3E3E3E";
    case "steel":
      return "#898E8C";
    case "ghost":
      return "#FFFFFF";
    case "ice":
      return "#78EFFF";
    default:
      return "#C9C9C9";
  }
}

function getLogo() {

}

onMounted(() => {
  obtenerUrlsPokemon(); // Llama a la función cuando la página se carga
});

async function obtenerUrlsPokemon() {
  for (let i = 1; i <= 50; i++) {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`);
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
}
async function obtenerUrlsPokemon2() {
  if (loading.value) {
    return;
  }

  const pokemonPerPage = 50;

  /* if (currentPage.value * pokemonPerPage > 1000) {
    return;
  } */

  loading.value = true;

  const start = (currentPage.value - 1) * pokemonPerPage + 51;
  const end = currentPage.value * pokemonPerPage + 50;

  for (let i = start; i <= end; i++) {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`);
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

  currentPage.value++;
  loading.value = false;
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
  padding: 0px 0px 0px 0px;
}

@font-face {
  font-family: "Pa ver";
  src: url("./fonts/Anta-Regular.ttf");
}

.img {
  width: 500px;
}

.ContainerBtnVerMas {
  background-color: transparent;
  padding: 0;
  display: flex;
  justify-content: center;
}

.BtnVerMas {
  outline: 0;
  border: 0;
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 140px;
  height: 50px;
  border-radius: 0.5em;
  box-shadow: 0 0.625em 1em 0 rgba(30, 143, 255, 0.35);
  overflow: hidden;
  padding: 0;
  cursor: pointer;
}

.BtnVerMas div {
  transform: translateY(0px);
  width: 100%;
}

.BtnVerMas,
.BtnVerMas div {
  transition: 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.BtnVerMas div span {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
}

.BtnVerMas div:nth-child(1) {
  background-color: rgb(0, 89, 255);
}

.BtnVerMas div:nth-child(2) {
  background-color: #21dc62;
}

.BtnVerMas:hover div {
  transform: translateY(-50px);
}

.BtnVerMas p {
  font-size: 25px;
  font-weight: bold;
  color: #ffffff;
  font-family: "Pa ver";
}

.BtnVerMas:active {
  transform: scale(0.95);
}

.imgTarjeta {
  width: 150px;
}

.BtnFiltrar {
  font-family: "Pa ver";
  color: #ffffff;
  font-size: 25px;
  background-color: transparent;
  border: none;
  cursor: pointer;
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
  width: 250px;
  border: none;
  font-family: "Pa ver";
}
.Tarjeta:hover {
  transform: scale(1.05);
  z-index: 1;
}
.ContainerTarjetas {
  display: flex;
  flex-wrap: wrap;
  gap: 85px;
  justify-content: space-around;
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
  justify-content: space-evenly;
  padding: 5px 0px;
  position: fixed;
  top: 0;
  width: 100%;
  align-items: center;
  flex-wrap: wrap;
  z-index: 2;
}

.main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0px 200px;
  margin: 110px 0px 50px 0px;
  gap: 55px;
  transition: margin-top 0.3s ease-in-out;
}

.Detalles {
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 100px;
  flex-direction: column;
  color: rgb(255, 255, 255);
  background-image: url('./assets/wallpaperflare.com_wallpaper (7).jpg');
}

.funcion {
  display: flex;
  width: 90%;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-around;
}

.ContainerOpcs {
  display: flex;
}

.datos {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  justify-content: space-between;
  width: 80%;
}

.containerDatos {
  width: 50%;
  display: flex;
  justify-content: center;
}

.datos h2 {
  display: flex;
  gap: 20px;
  align-items: center;
}

.cyberpunk-checkbox {
  position: absolute;
  appearance: none;
  width: 25px;
  height: 25px;
  border: 3px solid #000000;
  border-radius: 5px;
  background-color: transparent;
  display: inline-block;
  position: relative;
  cursor: pointer;
  margin: 0px 10px 0px 0px;
}

.cyberpunk-checkbox:before {
  content: "";
  background-color: #ffffff;
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0);
  width: 10px;
  height: 10px;
  border-radius: 3px;
  transition: all 0.3s ease-in-out;
}

.cyberpunk-checkbox:checked:before {
  transform: translate(-50%, -50%) scale(1);
}

.cyberpunk-checkbox-label {
  font-size: 18px;
  color: #fff;
  cursor: pointer;
  user-select: none;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}

.ContainerFiltrar {
  display: flex;
  gap: 25px;
  flex-wrap: wrap;
  padding: 0 5px;
  justify-content: center;
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
  justify-content: center;
  flex-wrap: wrap;
}

.img {
  width: 400px;
}

.Types {
  display: flex;
  width: 100%;
  justify-content: center;
  gap: 65px;
  flex-wrap: wrap;
}

.types {
  padding: 10px;
  border-radius: 10px;
  color: white;
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
  padding: 8px;
  border-radius: 15px;
}

.name {
  font-size: 100px;
  margin: 0;
}

.name::first-letter {
  text-transform: uppercase;
}

.funcionPart1 {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  align-items: center;
}

.input-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  transition: 0.5s ease-in-out;

  color: #fff;
}

.input {
  border-style: none;
  height: 50px;
  width: 0px;
  display: flex;
  align-content: center;
  outline: none;
  border-radius: 100%;
  transition: 0.5s ease-in-out;
  background-color: rgb(0, 89, 255);
  padding: 0px 12px 0px 0px;
}

.input::placeholder,
.input {
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
  font-size: 17px;
}

.input::placeholder {
  color: #8f8f8f;
}

.icon {
  display: flex;
  gap: 15px;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  /*   width: 150px; */
  height: 50px;
  outline: none;
  border-style: none;
  border-radius: 50%;
  pointer-events: painted;
  background-color: transparent;
  transition: 0.2s linear;
  color: #ffffff;
  font-size: 25px;
  font-family: "Pa ver";
}

.ConatinerPesoAltura {
  display: flex;
  width: 100%;
  justify-content: space-around;
  flex-wrap: wrap;
}

.ContainerAltura {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
  /* background-color: rgb(0, 89, 255); */
  padding: 10px;
  border-radius: 10px;
}

.ContainerPeso {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
  /* background-color: rgb(0, 89, 255); */
  padding: 10px;
  border-radius: 10px;
}

.ContainerAltura h2 {
  margin: 0;
}

.ContainerPeso h2 {
  margin: 0;
}

.icon:focus ~ .input,
.input:focus {
  box-shadow: none;
  width: 250px;
  border-radius: 0px;
  background-color: transparent;
  border-bottom: 2px solid #ffffff;
  transition: all 500ms cubic-bezier(0, 0.11, 0.35, 2);
}

.progress-bar {
  width: 100%;
  height: 10px;
  background-color: #ccc;
  margin-top: 5px;
}

.progress {
  height: 100%;
  background-color: #4caf50;
  transition: width 1s ease-in-out;
}

#Plus {
  font-size: 35px;
}

#LogoFiltrar {
  position: relative;
  height: 18px;
  right: 30px;
}
.Select {
  font-family: "Pa ver";
  background-color: rgb(0, 89, 255);
  border: none;
  font-size: 25px;
  color: white;
  text-transform: capitalize;
}
.name{
  color: white;
}
@media screen and (max-width: 1000px) {
  .containerDatos {
    width: 100%;
  }
  .main {
    padding: 0px 50px;
  }
}
@media screen and (max-width: 495px) {
  .Detalles {
    padding-top: 35%;
    background-size: cover;
  }
  .main {
    margin-top: 35%;
  }
}
@media screen and (max-width: 1300px) {
  .main {
    padding: 0px 100px;
  }
}
@media screen and (max-width: 710px) {
  .main {
    margin-top: 25%;
  }
}
@media screen and (max-width: 580px) {
  .main {
    margin-top: 35%;
  }
}
@media screen and (max-width: 475px) {
  .main {
    margin-top: 55%;
  }
  .name {
    font-size: 75px;
  }
}
</style>
