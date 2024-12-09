<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";
const details = ref([]);
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
instance.interceptors.request.use(function (config) {
    // Do something before request is sent
    console.log("Before request send"+config);
    
    return config;
  }, function (error) {
    // Do something with request error
    return Promise.reject(error);
  });
   // Add a response interceptor
   instance.interceptors.response.use(
    function (response) {
      console.log("Success"+ response);
     // const addNew={id:340,title:"dffdhgh fhf d",completed:false}
      const addOne=[{id:201,title:"dffdhgh fhf d",completed:true},
      {id:202,title:"esffdffdhgh fdd",completed:false},
      {id:203,title:"rwetreytrhydffdhghd",completed:true}
      ]
     // response.data.push(addNew)
     response.data=[...response.data,...addOne]
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
      details.value.forEach(i => {
    i.completed = i.completed ? '✔️' : '⤫';
});

    })
    .catch((err) => {
      console.log("Error : ");
    });
 
});
</script>

<template>
  
  <v-data-table :headers="headers" :items="details"></v-data-table>

  <!-- <pre>{{details }}</pre>-->
</template>
