<template>
  <v-app> 
    <v-app-bar >
      <v-toolbar-title class="headline">
        <span>ADUG Canberra Demo</span>
      </v-toolbar-title>
    </v-app-bar>

    <v-container class="fill-height">
      <v-main>
        <Earthquakes/>
      </v-main>
    </v-container>
  </v-app> 
</template>

<script>
import Earthquakes from './components/Earthquakes.vue';

export default {
  name: 'App',

  components: {
    Earthquakes,
  }
};
</script>
