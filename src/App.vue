
<script setup>
/* import {ref} from "vue" */
import axios from 'axios';
import { ref } from 'vue';

const img = ref()
const name = ref()
const Id = ref()
const StatHp = ref()
const Attack = ref()
const Defense = ref()
const SpecialAttack = ref()
const SpecialDefense = ref()
const Speed = ref()
const Weight = ref()
const Height = ref()
const Type0 = ref()
const Type1 = ref()
const tipo_pk = ref([])
const Mostrar = ref(false)
async function obtenerurlsPokemon() {
  /* let r = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=50&offset=0")
  console.log(r); */
  let data = await axios.get("https://pokeapi.co/api/v2/pokemon/448")
  console.log(data);
  img.value = data.data.sprites.other["official-artwork"].front_default
  name.value = data.data.name
  Id.value = data.data.id
  StatHp.value = data.data.stats[0].base_stat
  Attack.value = data.data.stats[1].base_stat
  Defense.value = data.data.stats[2].base_stat
  SpecialAttack.value = data.data.stats[3].base_stat
  SpecialDefense.value = data.data.stats[4].base_stat
  Speed.value = data.data.stats[5].base_stat
  Weight.value = data.data.weight
  Height.value = data.data.height
 data.data.types.forEach(element => {
    tipo_pk.value.push(element.type.name);
  });
/*   Type1.value = data.data.types[1].type.name */

  Mostrar.value = true
}


</script>

<template>
  <div class="body">
    <div class="nav">
      <h1>Pokedex</h1>
    </div>

    <div class="main">

    </div>












    <div class="Detalles" v-if="Mostrar">
      <div class="btn">
        <button @click="obtenerurlsPokemon()" class="botonPeticion">Peticion</button>
      </div>

      <div class="funcion" v-if="Mostrar">
        <div class="funcionPart1">
          <div class="nameContainer">
            <h2 class="name">{{ name }}</h2>
          </div>

          <div class="containerImg">

            <img :src="img" class="img">
            <h2 class="Id">#{{ Id }}</h2>
          </div>
          <div class="Types">
            <h2 v-for="(item, index) in tipo_pk" :key="index" class="types">{{ item }}</h2>
          </div>
        </div>
        <div>
          <div class="datos">


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

.botonPeticion {
  border-radius: 10px;
  padding: 15px;
  border: none;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
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

.types{
  background-color: rgb(0, 89, 255);
  padding: 10px;
  border-radius: 10px;
}


.Id {
  font-size: 250px;
  color: rgb(0, 89, 255);
  margin: 0;
}

.name {
  font-size: 100px;
  margin: 0;
}
.funcionPart1{
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>