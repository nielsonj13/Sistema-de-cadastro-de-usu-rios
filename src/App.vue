<script setup>
import { ref, onMounted } from 'vue'
// Forma correta e mais robusta:
import { supabase } from '@/libs/supabaseClient.js'
import UserForm from './components/UserForm.vue'
import UserList from './components/UserList.vue'

const usuarios = ref([])
const loading = ref(false)

// A função de buscar usuários agora mora aqui, no componente pai
async function fetchUsuarios() {
  try {
    loading.value = true
    const { data, error } = await supabase
      .from('usuarios')
      .select('*')
      .order('created_at', { ascending: false })

    if (error) throw error
    usuarios.value = data
  } catch (error) {
    console.error('Erro ao listar:', error.message)
  } finally {
    loading.value = false
  }
}

// Busca a lista inicial quando o aplicativo carrega
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
main {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  font-family: sans-serif;
}
</style>