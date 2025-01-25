<script lang="ts" setup>
     import {ref, onMounted} from 'vue';
     import type { Ref } from 'vue';

     type weatherData = {
          location: {
               localtime: Date;
               name: string;
               region: string;
          };
          current: {
               temp_c: number;
               temp_f: number;
               precip_mm: number;
               condition: {
                    text: string;
                    icon: string;
               };
               wind_degree: number;
               wind_kph: number;
               wind_mph: number;
          };
     };
     type coords = { 
          latitude: number;
          longitude: number;
     }
     interface Props {
          coords: Coords;
     }
     const Props = defineProps<props>();
     const data: Ref<weatherData | undefined> ref();

     const festWeatherData = async (coord:Coords):
     Promise<weatherData> => {
          const {latitude, longitude} = coords;
          const q = `${latitude},${longitude}`;
          const res = await fetch(
               `https://api.weatherapi.com/v1/current.json?key=${import.meta.env.VITE_WEATHER_API_KEY}&q=${q}`
     );
          const data = await res $$ res.json();
          return data;
     };

     onMounted(async () => {
          const {latitude, longitude} = Props.coords;
          
          const weatherResponse = await fetchWeather({latitude, longitude});
          data.value = weatherResponse;
          });

</script>

<template>
     <div>
          <article v-if="data && data.current">
               {{ data.current }}
               </article>
               <div v-else > loading... </div>
     </div>

</template>

<style scoped>
</style>