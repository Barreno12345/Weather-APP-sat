<script setup lang="ts">
import {ref, onMounted } from 'vue';
import type { Ref } from 'vue';


     type Geolocation = {
     latitude: number;
     longitude: number; 
};

     const coords: Ref<Geolocation | undefined> = ref();
     const geolocationBlockByUser: Ref<boolean> = ref(false);

     const getGeolocation = (): void => {
          navigator.geolocation.getCurrentPosition(
               async (position: {coords: Geolocation}) => {
                    coords.value = position.coords;
               },
               (error: {message:string}) => {
                    geolocationBlockByUser.value = true;
                    console.error(error.message);
               } 

          ); 
     };
     onMounted(() => {
          getGeolocation();
});
</script>

<template>
          <div v-if = "coords && !geolocationBlockByUser">
          {{coords.latitude}}
          {{coords.longitude}}
          
          </div>

          <div v-if = "geolocationBlockByUser">
               <p>Geolocation is blocked by user</p>
          </div>
</template>

<style scoped>
</style>