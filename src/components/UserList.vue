<script setup>
// Define que este componente espera receber uma propriedade chamada "users"
// que deve ser um Array.
defineProps({
  users: Array,
});
</script>

<template>
  <div class="user-list">
    <h2>Usuários Cadastrados</h2>
    <p v-if="users.length === 0">Nenhum usuário cadastrado ainda.</p>
    <ul v-else>
      <li v-for="user in users" :key="user.id">
        <strong>{{ user.nome }}</strong>
        <span>{{ user.email }}</span>
        <span>{{ user.telefone }}</span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.user-list {
  margin-top: 2rem;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #eee;
}
li:nth-child(odd) {
  background-color: #f9f9f9;
}
strong {
  color: #35495e;
}
</style>