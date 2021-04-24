<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
    <h1>Healthy Fruit</h1>
    </v-app-bar>
    <v-main>
      <div class="mt-10"><healthy-fruit :order="order"/></div>
    </v-main>
  </v-app>
</template>

<script>
import HealthyFruit from './components/HealthyFruit.vue';
import json from './data.json';

export default {
  name: 'App',
  data: () => ({
    order: json
  }),
  methods: {
  },
  mounted() {
    console.log(this.order);
  },
  components: { HealthyFruit }
};
</script>
