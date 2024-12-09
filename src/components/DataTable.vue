<script setup>
import axios from "axios";
import { onMounted, ref ,computed} from "vue";
import { IconCheck, IconX } from "@tabler/icons-vue";
const details = ref([]);
const search = ref("");
const selected=ref([])
const page=ref(1);
const itemsPerPage=ref(5)

const sortBy=[{key:'title',order:'asc'}]
const headers = ref([
  { title: "Id", key: "id" },
  { title: "Title", key: "title" },
  { title: "Completed", key: "completed" },
]);
onMounted(() => {

  const instance = axios.create({
    baseURL: "https://jsonplaceholder.typicode.com/todos",
    timeout: 5000,
  });
  // Add a request interceptor
  instance.interceptors.request.use(
    function (config) {
      // Do something before request is sent
      console.log("Before request send" + config);

      return config;
    },
    function (error) {
      // Do something with request error
      return Promise.reject(error);
    }
  );
  // Add a response interceptor
  instance.interceptors.response.use(
    function (response) {
      console.log("Success" + response);
      // const addNew={id:340,title:"dffdhgh fhf d",completed:false}
      const addOne = [
        { id: 201, title: "dffdhgh fhf d", completed: true },
        { id: 202, title: "esffdffdhgh fdd", completed: false },
        { id: 203, title: "rwetreytrhydffdhghd", completed: true },
      ];
      // response.data.push(addNew)
      response.data = [...response.data, ...addOne];
      return response;
    },
    function (error) {
      console.log("Error : " + err);

      return Promise.reject(error);
    }
  );

  instance
    .get()
    .then((res) => {
      details.value = res.data;
    
    })
    .catch((err) => {
      console.log("Error : ");
    });
});
</script>

<template>
  <v-sheet>
  <!--Search-->
    <v-text-field
      v-model="search"
      label="Search"
      prepend-inner-icon="mdi-magnify"
      variant="outlined"
      hide-details
      single-line
    ></v-text-field>

    <v-text-field
        :model-value="itemsPerPage"
        class="pa-2"
        label="Items per page"
        max="15"
        min="-1"
        type="number"
        hide-details
        @update:model-value="itemsPerPage = parseInt($event, 10)"
      ></v-text-field>

  <v-data-table 
  :headers="headers" 
  :items="details" 
  :loading="loading"
   v-model:sort-by="sortBy"
  v-model:page="page"
  v-model="selected"
  :search="search"
  :items-per-page="itemsPerPage"
  item-selectable="completed"
  return-object
  show-select
  >

    <!--template for tick-->
    <template v-slot:item.completed="{ item }">
      <div>
        <component :is="item.completed ? IconCheck : IconX" size="20" />
      </div>
    </template>

    <!--For pageNumber -->
    <template v-slot:bottom>
      <div class="text-center pt-2">
        <v-pagination
          v-model="page"
          :length="Math.ceil(details.length/itemsPerPage)"
        ></v-pagination>
      </div>
    </template>

  </v-data-table>
  <pre>{{ selected }}</pre>
</v-sheet>

</template>
