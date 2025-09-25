<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '@/libs/supabaseClient.js'
import UserForm from './components/UserForm.vue'
import UserList from './components/UserList.vue'

const usuarios = ref([])
const loading = ref(false)

async function fetchUsuarios() {
  try {
    loading.value = true
    const { data, error } = await supabase
    .from('usuarios')
    .select('*')
    .order('id', { ascending: false })

    if (error) throw error
    usuarios.value = data
  } catch (error) {
    console.error('Erro ao listar:', error.message)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchUsuarios()
})
</script>

<template>
  <main>
    <h1>Sistema de Cadastro</h1>
    <UserForm @userCreated="fetchUsuarios" />
    <UserList :users="usuarios" :loading="loading" />
  </main>
</template>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

main {
  max-width: 800px;
  margin: 3rem auto;
  padding: 2rem 1.5rem;
  font-family: 'Inter', sans-serif;
  background-color: #f9fafb;
  border-radius: 12px;
  box-shadow: 0 5px 15px rgb(0 0 0 / 0.05);
}

h1 {
  font-weight: 700;
  font-size: 2.2rem;
  color: #222;
  text-align: center;
  margin-bottom: 2rem;
  user-select: none;
}
</style>
