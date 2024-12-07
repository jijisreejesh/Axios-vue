<script setup>
import axios from "axios";
import { reactive } from "vue";
const item = reactive({
  name: "",
  data: {
    year: '',
    price: '',
    cpuModel: "",
    hardDiskSize: "",
  },
});
const instance = axios.create({
  baseURL: "https://api.restful-api.dev/objects",
  timeout: 1000,
});
const saveBtn = () => {
  console.log(item);

  if (item) {
   item.data.year=Number.parseInt(item.data.year);
   item.data.price=Number.parseInt(item.data.price)
    instance
      .post('',item)
      .then((res) => {
        console.log("Success", res.data);
      })
      .catch((err) => {
        console.log("error : " + err);
      });
  }
};
</script>

<template>
  <v-container>
    <v-row>
      <v-col>
        <v-text-field label="Enter the name" v-model="item.name"></v-text-field>
        <v-text-field
          label="Enter the year"
          type="number"
          v-model="item.data.year"
        ></v-text-field>
        <v-text-field label="Enter the price" type="number" v-model="item.data.price">
        </v-text-field>
        <v-text-field
          label="Enter the cpu-model"
          v-model="item.data.cpuModel"
        ></v-text-field>
        <v-text-field
          label="Enter the hard disk size"
          v-model="item.data.hardDiskSize"
        ></v-text-field>
        <v-btn @click="saveBtn"> Save </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
