<template>
    <div class="contenedor-resultado">
        <h2>Cotizacion</h2>
        <div class="resultado">
          <img
            :src="'https://cryptocompare.com/' + cotizacion.IMAGEURL"
            alt="imagen cripto"
          />
          <div>
            <p>
              El precio es de: <span>{{ cotizacion.PRICE }}</span>
            </p>
            <p>
              El precio mas alto del dia es:
              <span>{{ cotizacion.HIGHDAY }}</span>
            </p>
            <p>
              El precio mas bajo del dia: <span>{{ cotizacion.LOWDAY }}</span>
            </p>
            <p>
              Variacion ultimas 24hrs:
              <span>{{ cotizacion.CHANGEPCT24HOUR }}%</span>
            </p>
            <p>
              Ultima actualizacion: <span>{{ cotizacion.LASTUPDATE }}</span>
            </p>
          </div>
        </div>
      </div>
</template>

<script setup>
defineProps({
    cotizacion:{
        type: Object,
        required: true
    }
})
</script>
