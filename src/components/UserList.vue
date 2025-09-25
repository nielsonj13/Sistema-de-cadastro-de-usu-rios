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
/* Estilos da lista (pode copiar da resposta anterior) */
.card { padding: 20px; border: 1px solid #ccc; border-radius: 8px; }
ul { list-style: none; padding: 0; }
li { padding: 5px 0; }
</style>