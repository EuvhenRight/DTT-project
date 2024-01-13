<script setup lang="ts">
import BackToPages from '../components/Back-to-pages.vue'
import DynamicForm from '../components/Dynamic-form-edit.vue'

import { ref, onMounted } from 'vue'
import { useHousesStore } from '../stores/store'
import { useRouter, useRoute } from 'vue-router'
import type { HouseListing } from '../types/types'

const housesStore = useHousesStore()
const currentValues = ref({})
const currentImageValue = ref<string | null>(null)
const newEditedHouse = ref<HouseListing>()
const router = useRouter()
const route = useRoute()
const rawId = route.params.id as string
const id = parseInt(rawId, 10)
// Promises and timeouts
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms))

onMounted(async () => {
  // Fetch the house data by ID and set it as initial values
  await housesStore.getHouseById(id)

  if (housesStore.houseData[0].image) {
    currentImageValue.value = housesStore.houseData[0].image
  }

  currentValues.value = {
    streetName: housesStore.houseData[0].location.street,
    houseNumber: housesStore.houseData[0].location.houseNumber,
    numberAddition:
      housesStore.houseData[0].location.numberAddition === undefined
        ? ''
        : housesStore.houseData[0].location.numberAddition,
    zip: housesStore.houseData[0].location.zip,
    city: housesStore.houseData[0].location.city,
    constructionYear: housesStore.houseData[0].constructionYear,
    hasGarage: housesStore.houseData[0].hasGarage,
    description: housesStore.houseData[0].description,
    image: housesStore.houseData[0].image,
    bedrooms: housesStore.houseData[0].rooms.bedrooms,
    bathrooms: housesStore.houseData[0].rooms.bathrooms,
    size: housesStore.houseData[0].size,
    price: housesStore.houseData[0].price
  }
})

const submitForm = async (values) => {
  try {
    const formData = new FormData()
    const {
      price,
      bedrooms,
      bathrooms,
      size,
      streetName,
      houseNumber,
      numberAddition,
      zip,
      city,
      constructionYear,
      hasGarage,
      description,
      image
    } = values
    formData.append('price', price)
    formData.append('bedrooms', bedrooms)
    formData.append('bathrooms', bathrooms)
    formData.append('size', size)
    formData.append('streetName', streetName)
    formData.append('houseNumber', houseNumber)
    formData.append('numberAddition', numberAddition)
    formData.append('zip', zip)
    formData.append('city', city)
    formData.append('constructionYear', constructionYear)
    formData.append('hasGarage', hasGarage)
    formData.append('description', description)

    // Update the existing house data
    await housesStore.postEditHouse(id, formData)

    if (housesStore.editHouse) {
      newEditedHouse.value = housesStore?.editHouse
    }
    const formUploadImage = new FormData()
    formUploadImage.append('image', image, image.name)

    await delay(1000) // Adjust the delay as needed
    // Upload image after 1 second

    await housesStore.postEditUploadImage(housesStore.houseData[0].id, formUploadImage)
    // Redirect to house details with id
    router.push({ name: 'HouseDetails', params: { id: newEditedHouse.value?.id } })
  } catch (error) {
    console.error('Error during form submission:', error)
  }
}
const handleSubmit = (values) => {
  submitForm(values)
}
</script>
<template>
  <div class="background-listing">
    <div class="container-listing-view">
      <div class="wrapper-back-to-pages">
        <BackToPages />
        <h2 class="title-listing">Edit listing</h2>
      </div>
      <DynamicForm
        @on-submit="handleSubmit"
        :currentValues="currentValues"
        :currentImageValue="currentImageValue"
      />
    </div>
  </div>
</template>

<style scoped lang="scss">
.container-listing-view {
  margin: 0 auto;
  width: 1320px;
}
.background-listing {
  background-image: url(../assets/images/img_background@3x.png);
  background-repeat: no-repeat;
  background-size: cover;
}
.title-listing {
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
}
@media (min-width: 993px) and (max-width: 1321px) {
  .container-listing-view {
    width: 100%;
    padding: 0 20px;
  }
}
@media (max-width: 992px) {
  .container-listing-view {
    width: 100%;
    padding: 40px 20px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}
</style>