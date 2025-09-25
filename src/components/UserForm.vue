<script setup>
import { ref } from 'vue'
import { supabase } from '@/libs/supabaseClient.js'

// O 'emit' é a forma do componente filho "conversar" com o componente pai
const emit = defineEmits(['userCreated'])

const novoUsuario = ref({
  nome: '',
  email: '',
  senha: '',
  telefone: ''
})
const feedbackMessage = ref('')

async function cadastrarUsuario() {
  const { nome, email, senha, telefone } = novoUsuario.value
  if (!nome || !email || !senha) {
    feedbackMessage.value = 'Por favor, preencha nome, email e senha.'
    return
  }

  try {
    feedbackMessage.value = 'Cadastrando...'
    
    // Mesma lógica de inserção de antes
    const { error } = await supabase
      .from('usuarios')
      .insert([{ nome, email, senha, telefone }])

    if (error) throw error

    feedbackMessage.value = 'Usuário cadastrado com sucesso!'
    novoUsuario.value = { nome: '', email: '', senha: '', telefone: '' }
    
    // AQUI ESTÁ A MÁGICA: Avisa o componente pai que um novo usuário foi criado.
    emit('userCreated')

  } catch (error) {
    console.error('Erro ao cadastrar:', error.message)
    feedbackMessage.value = `Erro: ${error.message}`
  }
}
</script>

<template>
  <div class="card">
    <h2>Cadastrar Novo Usuário</h2>
    <form @submit.prevent="cadastrarUsuario">
      <input type="text" placeholder="Nome" v-model="novoUsuario.nome" required />
      <input type="email" placeholder="Email" v-model="novoUsuario.email" required />
      <input type="password" placeholder="Senha" v-model="novoUsuario.senha" required />
      <input type="text" placeholder="Telefone (opcional)" v-model="novoUsuario.telefone" />
      <button type="submit">Cadastrar</button>
    </form>
    <p v-if="feedbackMessage">{{ feedbackMessage }}</p>
  </div>
</template>

<style scoped>
/* Estilos do formulário (pode copiar da resposta anterior) */
.card { padding: 20px; border: 1px solid #ccc; border-radius: 8px; margin-bottom: 20px; }
input { display: block; width: 95%; padding: 8px; margin-bottom: 10px; }
button { padding: 10px 15px; background-color: #007bff; color: white; border: none; cursor: pointer; }
</style>