<template>
  <Navv @change="onChangeNav" />
  <main class="container my-4">
    <div v-if="TogleNav == 'arrays'">
      <h1>Tableaux</h1>
      <Image />
      <Models />
      <Price />
      <Type />
    </div>
    <Social v-if="TogleNav == 'social'" />
  </main>
</template>

<script>
import Navv from "./components/Nav.vue";
import Image from "./components/image.vue";
import Models from "./components/models.vue";
import Price from "./components/Price.vue"
import Type from "./components/type.vue"
import Social from "./components/social.vue";
import { ref } from "vue";
export default {
  name: "App",
  components: {
    Navv,
    Image,
    Models,
    Price,
    Type,
    Social,
  },
  setup() {
    const TogleNav = ref("arrays");
    const onChangeNav = (onglet) => {
      TogleNav.value = onglet;
    };
    
    return {
      TogleNav,
      onChangeNav
    };
  },
};
</script>

<style>
@import "bootstrap/dist/css/bootstrap.css";
@import "bootstrap-icons/font/bootstrap-icons.css";
</style>
