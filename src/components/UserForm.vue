<script setup>
import { ref } from 'vue';

// Define os "eventos" que este componente pode emitir (enviar) para o componente pai
const emit = defineEmits(['addUser']);

// Cria variáveis reativas para cada campo do formulário
const nome = ref('');
const email = ref('');
const senha = ref('');
const telefone = ref('');

// Função que será chamada quando o formulário for enviado
const handleSubmit = () => {
  // Cria um objeto com os dados do novo usuário
  const newUser = {
    id: Date.now(), // ID simples baseado no tempo atual
    nome: nome.value,
    email: email.value,
    senha: senha.value, // Em um projeto real, a senha deve ser criptografada!
    telefone: telefone.value,
  };

  // Emite o evento 'addUser' com os dados do novo usuário para o componente pai
  emit('addUser', newUser);

  // Limpa os campos do formulário após o envio
  nome.value = '';
  email.value = '';
  senha.value = '';
  telefone.value = '';
};
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <h2>Cadastrar Novo Usuário</h2>
    <div class="form-group">
      <label for="nome">Nome:</label>
      <input type="text" id="nome" v-model="nome" required />
    </div>
    <div class="form-group">
      <label for="email">Email:</label>
      <input type="email" id="email" v-model="email" required />
    </div>
    <div class="form-group">
      <label for="senha">Senha:</label>
      <input type="password" id="senha" v-model="senha" required />
    </div>
    <div class="form-group">
      <label for="telefone">Telefone:</label>
      <input type="tel" id="telefone" v-model="telefone" />
    </div>
    <button type="submit">Cadastrar</button>
  </form>
</template>

<style scoped>
form {
  margin-bottom: 2rem;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
}
.form-group {
  margin-bottom: 1rem;
}
label {
  display: block;
  margin-bottom: 5px;
}
input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  background-color: #42b983;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}
button:hover {
  background-color: #36a374;
}
</style>