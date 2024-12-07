<script setup>
import { ref, watch } from "vue";
import axios from "axios";
const meals = ref([]);
const search = ref("");
const filteredMeals = ref([]);
watch(search, async (newVal) => {
  console.log(newVal);
  filteredMeals.value = meals.value.filter((i) => {
    return i.strMeal.toLowerCase().includes(newVal.toLowerCase());
  });
});

const instance = axios.create({
  baseURL: "https://www.themealdb.com/api/json/v1/1/search.php?s=",
  timeout: 1000,
  //   headers: {'X-Custom-Header': 'foobar'}
});
instance.get("").then((res) => {
  meals.value = res.data.meals;
  filteredMeals.value = meals.value;
  console.log(filteredMeals.value);
});
</script>

<template>
  <v-card>
    <template v-slot:text>
      <v-text-field
        class="mx-auto w-50"
        v-model="search"
        label="Search"
        append-inner-icon="mdi-magnify"
        variant="outlined"
        single-line
      ></v-text-field>
    </template>

    <v-container>
      <v-row justify="space-between" align="start" class="mb-6">
        <v-col cols="6" md="3" v-for="meal in filteredMeals" :key="meal.idMeal">
          <v-card>
            <v-img :src="meal.strMealThumb" aspect-ratio="16/9" cover></v-img>
            <v-card-title class="text-center">{{ meal.strMeal }}</v-card-title>
            <v-card-text class="text-center">
              Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi,
              ratione debitis quis est labore voluptatibus!
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</template>
