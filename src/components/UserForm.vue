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
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

.card {
  padding: 30px 25px;
  border: 1px solid #ddd;
  border-radius: 10px;
  margin-bottom: 25px;
  background: #fff;
  box-shadow: 0 3px 8px rgb(0 0 0 / 0.05);
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
  font-family: 'Inter', sans-serif;
}

h2 {
  font-weight: 600;
  font-size: 1.5rem;
  color: #222;
  margin-bottom: 20px;
  text-align: center;
}

form {
  display: flex;
  flex-direction: column;
}

input {
  display: block;
  width: 100%;
  padding: 12px 14px;
  margin-bottom: 15px;
  border: 1.8px solid #ccc;
  border-radius: 6px;
  font-size: 1rem;
  transition: border-color 0.3s ease;
}

input:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

button {
  padding: 12px 0;
  background-color: #007bff;
  color: white;
  font-size: 1.1rem;
  font-weight: 600;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.25s ease;
}

button:hover:not(:disabled) {
  background-color: #0056b3;
}

button:disabled {
  background-color: #aaa;
  cursor: not-allowed;
}

p {
  margin-top: 15px;
  font-size: 0.9rem;
  text-align: center;
  padding: 8px 12px;
  border-radius: 6px;
}

/* Feedback de erro */
p:empty {
  display: none;
}

/* Mensagens de erro */
p[v-cloak][style*="Erro"] {
  background-color: #fdecea;
  color: #b00020;
  border: 1px solid #b00020;
}

/* Mensagens de sucesso */
p[v-cloak][style*="sucesso"], p[v-cloak][style*="Sucesso"], p[v-cloak][style*="sucesso!"] {
  background-color: #e6f4ea;
  color: #2f855a;
  border: 1px solid #2f855a;
}

/* Responsividade */
@media (max-width: 480px) {
  .card {
    max-width: 90%;
    padding: 20px 15px;
  }

  input, button {
    font-size: 1rem;
  }
}
</style>
