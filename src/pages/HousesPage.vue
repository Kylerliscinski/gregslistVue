<script setup>
import { computed, onMounted } from "vue";
import { AppState } from "../AppState.js";
import { housesService } from "../services/HousesService.js";
import Pop from "../utils/Pop.js";
import HouseCard from "../components/HouseCard.vue";
import HouseFormModal from "../components/HouseFormModal.vue";

const houses = computed(() => AppState.houses)
const account = computed(() => AppState.account)

async function getHouses(){
  try {
    await housesService.getHouses()
  } catch (error) {
    Pop.error(error)
  }
}

onMounted(() => {
  getHouses()
})

</script>


<template>
  <div class="container">
    <section class="row">
      <div class="col-12 mt-3 d-flex align-items-center gap-4">
        <h1>Houses</h1>
        <button v-if="account" class="btn btn-outline-dark" title="Create a new house listing!" data-bs-toggle="modal"
          data-bs-target="#houseFormModal">
          <i class="mdi mdi-plus"></i>
        </button>
      </div>
    </section>

    <section class="row">
      <div v-for="house in houses" :key="house.id" class="col-12 mb-3">
        <HouseCard :house="house" />
      </div>
    </section>
  </div>

  <HouseFormModal />
</template>


<style scoped></style>