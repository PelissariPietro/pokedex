<template>
  <router-view class="bg-image" />
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
});
</script>

<style>
.bg-image {
  background-image: url(./assets/images/pxfuel.jpg);
  background-repeat: no-repeat;
  background-size: auto;
  background-attachment: fixed;
}
</style>
