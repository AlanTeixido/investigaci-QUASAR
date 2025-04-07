<template>
    <q-page class="q-pa-md">
      <q-form @submit.prevent="handleSubmit" class="q-gutter-md">
  
        <q-input v-model="form.name" label="Nom" filled required />
        <q-input v-model="form.email" label="Correu electrònic" type="email" filled required />
        <q-input v-model="form.phone" label="Telèfon" filled />
        <q-input v-model="form.website" label="Web personal" filled />
  
        <q-select
          v-model="form.company"
          :options="companyOptions"
          label="Empresa"
          filled
          emit-value
          map-options
          required
        />
  
        <q-btn label="Enviar" type="submit" color="primary" />
        <q-btn label="Cancel·lar" flat @click="resetForm" />
      </q-form>
    </q-page>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import { useRouter } from 'vue-router'
  import { Notify } from 'quasar'
  
  const router = useRouter()
  
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
  
  function handleSubmit () {
    console.log('Usuari creat:', form.value)
    Notify.create({
      message: 'Usuari creat correctament!',
      color: 'positive'
    })
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
  