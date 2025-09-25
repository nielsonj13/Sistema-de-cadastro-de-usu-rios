<script setup>
import { ref } from 'vue'
import { supabase } from '@/libs/supabaseClient.js'

const emit = defineEmits(['userCreated'])

const novoUsuario = ref({
  nome: '',
  email: '',
  senha: '',
  telefone: ''
})
const feedbackMessage = ref('')
const loading = ref(false)

async function cadastrarUsuario() {
  const { nome, email, senha, telefone } = novoUsuario.value
  if (!nome || !email || !senha) {
    feedbackMessage.value = 'Por favor, preencha nome, email e senha.'
    return
  }

  loading.value = true
  feedbackMessage.value = 'Verificando dados...'

  try {
    // PASSO 1: VERIFICAR SE O E-MAIL JÁ EXISTE
    const { data: emailExistente, error: emailError } = await supabase
      .from('usuarios')
      .select('email')
      .eq('email', email) // .eq() significa 'equals' (igual a)

    if (emailError) {
      // Se der um erro na verificação, pare aqui
      throw new Error(`Erro ao verificar e-mail: ${emailError.message}`)
    }

    if (emailExistente && emailExistente.length > 0) {
      // Se encontrou algum resultado, o e-mail já existe
      feedbackMessage.value = 'Este e-mail já está cadastrado. Tente outro.'
      loading.value = false
      return // Para a execução da função
    }

    // PASSO 2: SE O E-MAIL NÃO EXISTE, PROSSIGA COM A INSERÇÃO
    feedbackMessage.value = 'Cadastrando...'
    const { error: insertError } = await supabase
      .from('usuarios')
      .insert([{ nome, email, senha, telefone }]) // AINDA INSEGURO POR CAUSA DA SENHA!

    if (insertError) {
      throw insertError
    }

    feedbackMessage.value = 'Usuário cadastrado com sucesso!'
    novoUsuario.value = { nome: '', email: '', senha: '', telefone: '' }
    emit('userCreated')

  } catch (error) {
    console.error('Erro ao cadastrar:', error.message)
    feedbackMessage.value = `Erro: ${error.message}`
  } finally {
    loading.value = false
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
      <button type="submit" :disabled="loading">
        {{ loading ? 'Processando...' : 'Cadastrar' }}
      </button>
    </form>
    <p v-if="feedbackMessage">{{ feedbackMessage }}</p>
  </div>
</template>

<style scoped>
/* Estilos ... */
.card { padding: 20px; border: 1px solid #ccc; border-radius: 8px; margin-bottom: 20px; }
input { display: block; width: 95%; padding: 8px; margin-bottom: 10px; }
button { padding: 10px 15px; background-color: #007bff; color: white; border: none; cursor: pointer; }
button:disabled { background-color: #aaa; }
</style>