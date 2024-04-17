<script setup>
import { computed } from "vue";
import { House } from "../models/House.js";
import { AppState } from "../AppState.js";
import Pop from "../utils/Pop.js";
import { logger } from "../utils/Logger.js";
import { housesService } from "../services/HousesService.js";



defineProps({
  house: House
})

const account = computed(() => AppState.account)

async function destroyHouse(houseId){
  try {
    const wantsToDestroy = await Pop.confirm("Are you sure you want top delete this listing?")
    if(!wantsToDestroy) return
    logger.log('Destroying house', houseId)
    await housesService.destroyHouse(houseId)
  } catch (error) {
    Pop.error(error)
  }
}
</script>


<template>
  <div class="bg-light rounded shadow d-flex">
    <img :src="house.imgUrl" alt="image of cool house" class="house-img img-fluid rounded-start">
    <div class="p-3">
      <h2>Beds: {{ house.bedrooms }} | Bath: {{ house.bathrooms }}</h2>
      <h3>Floors: {{ house.levels }}</h3>
      <h3>${{ house.price }}</h3>
      <p>{{ house.description }}</p>
      <button v-if="house.creatorId == account?.id" @click="destroyHouse(house.id)" class="btn btn-outline-danger">
        <i class="mdi mdi-home-off"></i>
      </button>
    </div>
  </div>
</template>


<style lang="scss" scoped>
.house-img {
  width: 40%;
  height: 40vh;
  object-fit: cover;
}
</style>