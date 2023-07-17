<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-red-9">
      <q-toolbar>
        <q-toolbar-title> VueQuasar Pokedex </q-toolbar-title>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "MainLayout",
});
</script>
