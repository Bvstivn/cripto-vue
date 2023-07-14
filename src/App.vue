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

        <input type="submit" value="Cotizar" @click="obtenerCotizacion"/>
      </form>

      <div v-if="mostrarResultado" class="contenedor-resultado">
        <h2>Cotizacion</h2>
        <div class="resultado">
          <img :src="'https://cryptocompare.com/'+cotizacion.IMAGEURL" alt="imagen cripto">
          <div>
            <p>El precio es de: <span>{{ cotizacion.PRICE }}</span></p>
            <p>El precio mas alto del dia es: <span>{{ cotizacion.HIGHDAY }}</span></p>
            <p>El precio mas bajo del dia: <span>{{ cotizacion.LOWDAY }}</span></p>
            <p>Variacion ultimas 24hrs: <span>{{ cotizacion.CHANGEPCT24HOUR }}%</span></p>
            <p>Ultima actualizacion: <span>{{ cotizacion.LASTUPDATE }}</span></p>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
//Vue
import { ref, reactive, onMounted, computed } from "vue";
//Components
import Alerta from "../src/components/Alerta.vue";

const monedas = ref([
  { codigo: "USD", texto: "Dolar de Estados Unidos" },
  { codigo: "MXN", texto: "Peso Mexicano" },
  { codigo: "EUR", texto: "Euro" },
  { codigo: "GBP", texto: "Libra Esterlina" },
  { codigo: "CLP", texto: "Peso Chileno" },
]);

const criptomonedas = ref([]);
const error = ref("");
const cotizar = reactive({
  moneda: "",
  criptomoneda: "",
});
const cotizacion = ref({});

onMounted(() => {
  fetch(
    "https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD"
  ).then((respuesta) =>
    respuesta.json().then(({ Data }) => {
      criptomonedas.value = Data;
    })
  );
});

const cotizarCripto = () => {
  //Validar que cotizar este lleno
  if (Object.values(cotizar).includes("")) {
    error.value = "Todos los campos son obligatorios";
    return;
  } 
  error.value = '';
  obtenerCotrizacion();
};

const obtenerCotizacion = async () => {
  const { moneda, criptomoneda } = cotizar;
  const url = `https://min-api.cryptocompare.com/data/pricemultifull?fsyms=${criptomoneda}&tsyms=${moneda}`;

  const respuesta = await fetch(url);
  const data = await respuesta.json();

  cotizacion.value = data.DISPLAY[criptomoneda][moneda];

}

const mostrarResultado = computed(()=>{
  return Object.values(cotizacion.value).length > 0;
})

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
