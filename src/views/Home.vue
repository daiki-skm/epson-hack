<template>
  <div class="home">
    <HelloWorld />
    <v-text-field v-model="getUrl" placeholder="input here"></v-text-field>
  </div>
  
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  }
}
</script>
