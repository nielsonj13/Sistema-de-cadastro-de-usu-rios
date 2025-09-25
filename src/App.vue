<script setup>
import { ref } from 'vue';
import UserForm from './components/UserForm.vue';
import UserList from './components/UserList.vue';

// A lista de usuários será guardada aqui, no componente principal.
// Começa como uma lista vazia.
const users = ref([]);

// Esta função recebe o novo usuário (que veio do evento do UserForm)
// e o adiciona na nossa lista 'users'.
const handleNewUser = (newUser) => {
  users.value.push(newUser);
};
</script>

<template>
  <main>
    <h1>Sistema de Cadastro</h1>
    
    <UserForm @addUser="handleNewUser" />
    
    <UserList :users="users" />

  </main>
</template>

<style scoped>
main {
  font-family: sans-serif;
  max-width: 800px;
  margin: 50px auto;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  color: #2c3e50;
}
</style>