<template>

  <main class="container my-4">
  
      <Image />
       </main>
</template>

<script>

import Image from "./components/image.vue";
 
export default {
  name: "App",
  components: {
    Image,
    
  },
  
};
</script>

<style>
@import "bootstrap/dist/css/bootstrap.css";
@import "bootstrap-icons/font/bootstrap-icons.css";
</style>
