<template>
  <q-page class="row items-center justify-evenly">
    <numbers-component />
  </q-page>
</template>

<script setup lang="ts">
import NumbersComponent from 'components/NumbersComponent.vue';

defineOptions({
  name: 'IndexPage',
});
</script>
