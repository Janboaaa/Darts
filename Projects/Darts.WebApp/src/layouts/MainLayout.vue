<template>
  <q-layout view="lHh Lpr lFf">
    <app-bar-component />

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import AppBarComponent from 'components/AppBarComponent.vue';

defineOptions({
  name: 'MainLayout',
});
</script>
