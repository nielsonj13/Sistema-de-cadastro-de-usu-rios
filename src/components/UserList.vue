<script setup>
// 'defineProps' é como o componente declara os "argumentos" que ele espera receber do pai
const props = defineProps({
  users: {
    type: Array,
    required: true
  },
  loading: {
    type: Boolean,
    default: false
  }
})
</script>

<template>
  <div class="card">
    <h2>Usuários Cadastrados</h2>
    <div v-if="loading">Carregando lista...</div>
    <ul v-else-if="users.length > 0">
      <li v-for="usuario in users" :key="usuario.id">
        <strong>{{ usuario.nome }}</strong> - {{ usuario.email }}
      </li>
    </ul>
    <p v-else>Nenhum usuário cadastrado ainda.</p>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

.card {
  padding: 30px 25px;
  border: 1px solid #ddd;
  border-radius: 10px;
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

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 12px 15px;
  border-bottom: 1px solid #eee;
  font-size: 1rem;
  color: #444;
  transition: background-color 0.3s ease;
  cursor: default;
  border-radius: 6px;
}

li:hover {
  background-color: #f0f8ff;
}

div[v-cloak], p {
  text-align: center;
  font-size: 1rem;
  color: #666;
  padding: 15px 0;
}

/* Mensagem de carregando */
div[v-cloak] {
  font-style: italic;
  color: #007bff;
}

/* Mensagem de nenhum usuário */
p {
  font-style: italic;
  color: #888;
}

/* Responsividade */
@media (max-width: 480px) {
  .card {
    max-width: 90%;
    padding: 20px 15px;
  }

  li {
    padding: 10px 12px;
    font-size: 0.95rem;
  }
}
</style>
