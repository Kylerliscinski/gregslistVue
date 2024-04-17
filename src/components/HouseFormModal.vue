<script setup>
import { ref } from "vue";
import { logger } from "../utils/Logger.js";
import { housesService } from "../services/HousesService.js";
import { Modal } from "bootstrap";
import Pop from "../utils/Pop.js";


const editableHouseData = ref({
  bedrooms: 0,
  bathrooms: 0,
  levels: 0,
  year: 0,
  price: 0,
  description: '',
  imgUrl: ''
})

async function createHouse(){
  try {
    logger.log('creating house', editableHouseData)
    await housesService.createHouse(editableHouseData.value)

    editableHouseData.value = {
      bedrooms: 0,
      bathrooms: 0,
      levels: 0,
      year: 0,
      price: 0,
      description: '',
      imgUrl: ''
    }
    Modal.getOrCreateInstance('#houseFormModal').hide()
  } catch (error) {
    Pop.error(error)
  }
}

</script>


<template>
  <div class="modal fade" id="houseFormModal" tabindex="-1" aria-labelledby="houseFormModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="houseFormModalLabel">Create a new House Listing!</h1>
        </div>
        <div class="modal-body">
          <form @submit.prevent="createHouse()">
            <div class="form-floating mb-3">
              <input v-model="editableHouseData.bedrooms" type="number" class="form-control" id="houseBedrooms" placeholder="Bedrooms" required min="1" max="20">
              <label for="houseBedrooms">Bedrooms</label>
            </div>
            <div class="form-floating mb-3">
              <input v-model="editableHouseData.bathrooms" type="number" class="form-control" id="houseBathrooms" placeholder="Bathrooms" required min="1" max="20">
              <label for="houseBathrooms">Bathrooms</label>
            </div>
            <div class="form-floating mb-3">
              <input v-model="editableHouseData.levels" type="number" class="form-control" id="houseLevels" placeholder="Levels" required min="1" max="20">
              <label for="houseLevels">Levels</label>
            </div>
            <div class="form-floating mb-3">
              <input v-model="editableHouseData.year" type="number" class="form-control" id="houseYear" placeholder="Year" required min="1" max="20">
              <label for="houseYear">Year</label>
            </div>
            <div class="form-floating mb-3">
              <input v-model="editableHouseData.price" type="number" class="form-control" id="housePrice"
                placeholder="House Price" required>
              <label for="housePrice">House Price</label>
            </div>
            <div class="form-floating mb-3">
              <textarea v-model="editableHouseData.description" class="form-control"
                placeholder="Leave a description here" id="houseDescription" maxlength="500"></textarea>
              <label for="houseDescription">House Description</label>
            </div>
            <div class="form-floating mb-3">
              <input v-model="editableHouseData.imgUrl" type="url" class="form-control" id="houseImgUrl"
                placeholder="House Image" required maxlength="500">
              <label for="houseImgUrl">House Image</label>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<style lang="scss" scoped>

</style>