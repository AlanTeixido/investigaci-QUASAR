<template>
    <q-page class="q-pa-md">
      <q-card class="q-pa-md">
        <q-card-section>
          <div class="text-h6">{{ user.name }}</div>
          <div class="text-subtitle2">{{ user.email }}</div>
          <p><strong>Telèfon:</strong> {{ user.phone }}</p>
          <p><strong>Empresa:</strong> {{ user.company?.name }}</p>
          <p><strong>Web:</strong> {{ user.website }}</p>
          <q-btn label="Tornar" flat @click="router.back()" class="q-mt-md" />
        </q-card-section>
      </q-card>
    </q-page>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { useRoute, useRouter } from 'vue-router'
  import axios from 'axios'
  
  const route = useRoute()
  const router = useRouter()
  const user = ref({})
  
  onMounted(async () => {
    const res = await axios.get(`https://jsonplaceholder.typicode.com/users/${route.params.id}`)
    user.value = res.data
  })
  </script>
  