<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h1>Healthy Fruit</h1>
    </v-app-bar>
    <v-main>
      <div class="mt-15">
        <healthy-fruit :order="order" @orderSubmited="orderSubmited" />
      </div>
      <v-snackbar
        :timeout="timeout"
        :value="alert"
        color="success"
        absolute
        top
      >
        Order was submited successfully.
        <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="alert=false"
        >
          Close
        </v-btn>
      </template>
      </v-snackbar>
    </v-main>
  </v-app>
</template>

<script>
import HealthyFruit from "./components/HealthyFruit.vue";
import json from "./data.json";

export default {
  name: "App",
  data: () => ({
    order: json,
    alert: false,
    timeout: 2000,
  }),
  methods: {
    orderSubmited() {
      
      this.alert = true;
      setTimeout(() => {
        this.alert = false;
      },this.timeout)
    }
  },
  mounted() {},
  components: { HealthyFruit },
};
</script>
