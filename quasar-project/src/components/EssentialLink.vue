<template>
<!-- prova -->

<q-item
  v-for="(link, index) in linksList"
  :key="index"
  class="q-pa-sm"
  :to="link.link"
  target="_blank"
  tag="a"
  ></q-item>
</template>

<script setup>
//
</script>
