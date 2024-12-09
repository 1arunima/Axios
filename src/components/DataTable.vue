<script setup>
import axios from 'axios'
import { ref } from 'vue'
import { IconCheck, IconCircleDashedX} from '@tabler/icons-vue'

const details = ref([])
const search =ref('')
const page =ref(1)
const itemsPerPage=ref()
const loading =ref(false)
const sortBy=[{ key: 'title', order: 'asc' }]

const headers = ref([
  { title: "ID", value: "id", key: "id" },
  { title: "Title", value: "title" },
  { title: "Completed", value: "completed" },
])

const instance = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com/todos',
  timeout: 1000,
})

instance.interceptors.request.use(function(config) {
  console.log("Enter before");
  return config;
}, function(error) {
  return Promise.reject(error);
});

instance.interceptors.response.use(function(response) {
  console.log("Success", response);

  // Adding new data to the response
  const newData = { id: 201, title: "New Task", completed: false };
  response.data.push(newData);

  return response;
}, function(error) {
  console.log("Error occurred", error);
  return Promise.reject(error);
});

instance.get()
  .then((res) => {
    details.value = res.data;

  })
  .catch((err) => {
    console.log(err);
  })


  const fetchData = async () => {
  loading.value = true; // Start loading
  try {
    const res = await instance.get();
    details.value = res.data;
  } catch (err) {
    console.error("Error fetching data", err);
    alert("Failed to fetch data. Please try again.");
  } finally {
    loading.value = false; 
  }
};

// Fetch data on component mount
fetchData();
</script>


<template>
    <div>
      <h1>Data Table</h1>
  
      
      <v-text-field
        v-model="search"
        label="Search"
        prepend-inner-icon="mdi-magnify"
        variant="outlined"
        hide-details
        single-line
        class="mb-4"
      ></v-text-field>
  
     
      <v-data-table
        v-model="selected"
        :headers="headers"
        :items="details"
        :search="search"
        :items-per-page="itemsPerPage"
          v-model:sort-by="sortBy"
        v-model:page="page"
        item-value="name"
        item-selectable="completed"
        select-strategy="page"
        :loading="loading"
        return-object
        show-select
        class="elevation-1"
      >
      
        <template v-slot:top>
          <v-text-field
            v-model="itemsPerPage"
            type="number"
            label="Items per page"
            min="1"
            max="15"
            variant="outlined"
            class="pa-2"
          ></v-text-field>
        </template>
  
    
       
        <template v-slot:item.completed="{ item }">
          <component
            :is="item.completed ? IconCheck : IconCircleDashedX"
            size="20"
          />
        </template>
  
        <!-- Pagination -->
        <template v-slot:bottom>
          <div class="text-center pt-2">
            <v-pagination
              v-model="page"
              :length="Math.ceil(details.length / itemsPerPage)"
            ></v-pagination>
          </div>
        </template>
      </v-data-table>
      <pre>{{ selected }}</pre>
    </div>
  </template>