
<script setup>

import {ref,watch} from 'vue'
import axios from 'axios'
import instance from '@/Services/AxiosInstance'


const datas =ref('')
const search = ref('')
const filteredMeals = ref([]);





instance
.get('search.php?s=')
.then((response)=>{
  datas.value=response.data.meals || []
  filteredMeals.value =datas.value
})
.catch((error)=> console.error('error fetching data:' ,error))

watch(search, (newValue) => {
  filteredMeals.value = datas.value.filter((meal) =>
    meal.strMeal.toLowerCase().includes(newValue.toLowerCase())
  );
});

</script>
<template>
    
<v-container>
    <h1 class="text-center mb-6 ">  Enjoy Eating</h1>

<v-row justify="center" class="d-flex mb-4">
  <v-col cols="12" md="4">
    <v-text-field
      v-model="search"
      label="Search Meals"
      outlined
      dense
      clearable
    append-inner-icon="mdi-magnify"
    ></v-text-field>
  </v-col>
</v-row>
    <v-row justify="space-between" align="start" class="mb-6">
      <v-col
        cols="6"
        md="3"
        v-for="meal in filteredMeals"
        :key="meal.idMeal"
      >
        <v-card class="text-center">
          <v-img
            :src="meal.strMealThumb"
            aspect-ratio="16/9"
            cover
          ></v-img>
          <v-card-title>{{ meal.strMeal }}</v-card-title>
          <v-card-text>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus!
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

