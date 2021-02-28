<template>
  <v-app>
    <v-app-bar app color="black" dark>
      <h1>NeosVR</h1>
      <h2>japan</h2>
    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
