<template>
  <div class="home">
    <GooglePhoto msg="Welcome to Your Vue.js App"/>
  </div>
  
</template>

<script>
import GooglePhoto from '@/components/googlePhoto.vue'

export default {
  name: 'Home',
  components: {
    GooglePhoto
  }
}
</script>
