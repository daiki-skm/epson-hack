<template>
  <div class="home">
    <HelloWorld />
    <v-btn @click="test()">TEST</v-btn>
    <img v-if="show" :src="textareavalue"> 
    <v-text-field v-model="getUrl" placeholder="input here"></v-text-field>
  </div>
  
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  }, 
  data: ()=> ({
    getUrl: 'https://photos.app.goo.gl/6nNMr6gmhpHZnT8H8',
    textareavalue: "",
    show: true,
  }),
  methods:{
    test(){
      this.textareavalue = "https://chart.googleapis.com/chart?cht=qr&chs=200x200&chl=" + this.getUrl + "&choe=UTF-8QRCode"  
    }
  }
}
</script>
