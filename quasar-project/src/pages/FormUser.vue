<template>
    <q-page class="q-pa-md">
      <q-form @submit.prevent="handleSubmit" class="q-gutter-md">
  
        <q-input
          v-model="form.name"
          label="Nom"
          filled
          :rules="[val => !!val || 'El nom és obligatori']"
          required
        />
  
        <q-input
          v-model="form.email"
          label="Correu electrònic"
          type="email"
          filled
          :rules="[val => !!val || 'El correu és obligatori']"
          required
        />
  
        <q-input
          v-model="form.phone"
          label="Telèfon"
          filled
        />
  
        <q-input
          v-model="form.website"
          label="Web personal"
          filled
        />
  
        <q-select
          v-model="form.company"
          :options="companyOptions"
          label="Empresa"
          filled
          emit-value
          map-options
          :rules="[val => !!val || 'Selecciona una empresa']"
          required
        />
  
        <div class="q-gutter-sm">
          <q-btn label="Enviar" type="submit" color="primary" />
          <q-btn label="Cancel·lar" flat @click="resetForm" />
        </div>
      </q-form>
    </q-page>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { useRouter, useRoute } from 'vue-router'
  import axios from 'axios'
  import { Notify } from 'quasar'
  
  const router = useRouter()
  const route = useRoute()
  
  const form = ref({
    name: '',
    email: '',
    phone: '',
    website: '',
    company: null
  })
  
  const companyOptions = [
    { label: 'Google', value: 'Google' },
    { label: 'Apple', value: 'Apple' },
    { label: 'Amazon', value: 'Amazon' },
    { label: 'Microsoft', value: 'Microsoft' },
  ]
  
  const isEditing = ref(false)
  
  onMounted(async () => {
    if (route.query.id) {
      isEditing.value = true
      try {
        const res = await axios.get(`https://jsonplaceholder.typicode.com/users/${route.query.id}`)
        const user = res.data
        form.value = {
          name: user.name,
          email: user.email,
          phone: user.phone,
          website: user.website,
          company: user.company?.name ?? null
        }
      } catch {
        Notify.create({ message: 'No s\'ha pogut carregar l\'usuari', color: 'negative' })
        router.push('/')
      }
    }
  })
  
  function handleSubmit () {
    if (isEditing.value) {
      console.log('Usuari EDITAT:', form.value)
      Notify.create({ message: 'Usuari editat correctament!', color: 'primary' })
    } else {
      console.log('Usuari creat:', form.value)
      Notify.create({ message: 'Usuari creat correctament!', color: 'positive' })
    }
  
    router.push('/')
  }
  
  function resetForm () {
    form.value = {
      name: '',
      email: '',
      phone: '',
      website: '',
      company: null
    }
  }
  </script>
  