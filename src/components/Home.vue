
<script setup>

import {ref,watch} from 'vue'
import axios from 'axios'


const datas =ref('')
const search = ref('')
const filteredMeals = ref([]);
// const instance = axios.create({
//   baseURL: 'https://www.themealdb.com/api/json/v1/1/search.php?s=',
//   timeout: 1000,
 
// });
// instance.get('')
//   .then(response => {
//     console.log(response.data.meals);
//     datas.value=response.data.meals
//   })
//   .catch(error => {
//     console.error('Error fetching data:', error);
//   });



  axios
  .get('https://www.themealdb.com/api/json/v1/1/search.php?s=')
  .then((response) => {
    datas.value = response.data.meals || [];
    filteredMeals.value = datas.value; // Initially show all meals
  })
  .catch((error) => console.error('Error fetching data:', error));

// Watch for changes in the search term and filter meals locally
watch(search, (newValue) => {
  filteredMeals.value = datas.value.filter((meal) =>
    meal.strMeal.toLowerCase().includes(newValue.toLowerCase())
  );
});

</script>
<template>
    
<v-container>
    <h1>  Enjoyyyyyy Eating</h1>

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

