<template>
  <div class="contenedor">
    <h1 class="titulo">Cotizador de <span>Criptomonedas</span></h1>
    <div class="contenido">
      <Alerta v-if="error">
        {{ error }}
      </Alerta>
      <form @submit.prevent="cotizarCripto" class="formulario">
        <div class="campo">
          <label for="moneda">Moneda:</label>
          <select v-model="cotizar.moneda" id="moneda">
            <option value="">--Selecciona</option>
            <option v-for="moneda in monedas" :value="moneda.codigo">
              {{ moneda.texto }}
            </option>
          </select>
        </div>
        <div class="campo">
          <label for="cripto">Criptomoneda:</label>
          <select v-model="cotizar.criptomoneda" id="cripto">
            <option value="">--Selecciona</option>
            <option
              v-for="criptomoneda in criptomonedas"
              :value="criptomoneda.CoinInfo.Name"
            >
              {{ criptomoneda.CoinInfo.FullName }}
            </option>
          </select>
        </div>

        <input type="submit" value="Cotizar" @click="obtenerCotizacion" />
      </form>

      <Spinner v-if="cargando" />

      <Cotizacion :cotizacion = cotizacion v-if="mostrarResultado"/>
    </div>
  </div>
</template>

<script setup>
//Vue
import { ref, reactive } from "vue";
//Components
import Alerta from "../src/components/Alerta.vue";
import Spinner from "../src/components/Spinner.vue";
import Cotizacion from "./components/Cotizacion.vue";
//Composables
import useCripto from "./composables/useCripto";

//Composaboles
const { monedas, criptomonedas, cargando, obtenerCotizacion, cotizacion, mostrarResultado } = useCripto();

const error = ref("");
  const cotizar = reactive({
    moneda: "",
    criptomoneda: "",
  });




const cotizarCripto = () => {
  //Validar que cotizar este lleno
  if (Object.values(cotizar).includes("")) {
    error.value = "Todos los campos son obligatorios";
    return;
  }
  error.value = "";
  obtenerCotizacion(cotizar);
};

</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
