<script setup>
import { ref, reactive} from 'vue';
import axios from 'axios'

// Reactive object to hold item details
const item = reactive({
  name: '',
  data: {
    year: '',
    price: '',
    model: '',
    size: '',
  },
});
const savedItem = ref(null);
const instance = axios.create({
    baseURL:'https://api.restful-api.dev/objects',
    timeout:1000,
})



// Save function
const save = () => {
    if(item){
  instance.post('',item)
  .then((res)=>{
    console.log("success",res.data);
    savedItem.value = res.data;
    
  })
  .catch((err)=>{
    console.log("eror",err);
    
  })
}
};
</script>

<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6">
        
        <v-text-field
          label="Enter the name"
          v-model="item.name"
          outlined
          dense
        ></v-text-field>

      
        <v-text-field
          label="Enter the year"
          v-model="item.data.year"
          type="number"
          outlined
          dense
        ></v-text-field>

        <v-text-field
          label="Enter the price"
          v-model="item.data.price"
          type="number"
          outlined
          dense
        ></v-text-field>

        
        <v-text-field
          label="Enter the model"
          v-model="item.data.model"
          outlined
          dense
        ></v-text-field>

        
        <v-text-field
          label="Enter the size"
          v-model="item.data.size"
          outlined
          dense
        ></v-text-field>

      
        <v-btn @click="save" class="mt-4" color="primary">
          Save
        </v-btn>
      </v-col>
      <v-col cols="12" md="6">
  <div v-if="savedItem">
    <p><strong>Saved Data:</strong></p>
    <pre>{{ savedItem }}</pre> <!-- Display the saved data -->
  </div>
</v-col>
    </v-row>
</v-container>
</template>