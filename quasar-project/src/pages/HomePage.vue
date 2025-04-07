<template>
    <q-page class="q-pa-md">
        <q-input v-model="search" label="Cerca usuari..." class="q-mb-md" filled debounce="300" />

        <q-table
        :rows="filteredUsers"
        :columns="columns"
        row-key="id"
        flat
        bordered
        @row-click="goToDetail"
      >
        <template v-slot:body-cell-actions="props">
          <q-td :props="props">
            <q-btn
              icon="delete"
              color="negative"
              flat
              round
              @click.stop="openConfirmDialog(props.row)"
            />
          </q-td>
        </template>
      </q-table>

      <q-dialog v-model="showDialog">
        <q-card>
          <q-card-section>
            <div class="text-h6">Vols eliminar aquest usuari?</div>
            <div>{{ selectedUser?.name }}</div>
          </q-card-section>
      
          <q-card-actions align="right">
            <q-btn flat label="Cancel·lar" v-close-popup />
            <q-btn flat label="Eliminar" color="negative" @click="deleteUser" />
          </q-card-actions>
        </q-card>
      </q-dialog>

      
    </q-page>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'
import { Notify } from 'quasar'


function goToDetail(row) {
  router.push(`/user/${row.id}`)
}


const users = ref([])
const search = ref('')
const router = useRouter()
const showDialog = ref(false)
const selectedUser = ref(null)

function openConfirmDialog(user) {
  selectedUser.value = user
  showDialog.value = true
}

function deleteUser() {
  users.value = users.value.filter(u => u.id !== selectedUser.value.id)
  Notify.create({ message: 'Usuari eliminat', color: 'negative' })
  showDialog.value = false
}



const columns = [
  { name: 'name', label: 'Nom', field: 'name', sortable: true },
  { name: 'email', label: 'Email', field: 'email', sortable: true },
  { name: 'company', label: 'Empresa', field: row => row.company.name, sortable: true },
  { name: 'actions', label: 'Accions', field: 'id', sortable: false }
]


const filteredUsers = computed(() => {
    const term = search.value.toLowerCase()
    return users.value.filter(user =>
        user.name.toLowerCase().includes(term) ||
        user.email.toLowerCase().includes(term)
    )
})

onMounted(async () => {
  try {
    const res = await axios.get('https://jsonplaceholder.typicode.com/users')
    users.value = res.data
  } catch {
    Notify.create({ message: 'Error carregant usuaris', color: 'negative' })
  }
})

</script>