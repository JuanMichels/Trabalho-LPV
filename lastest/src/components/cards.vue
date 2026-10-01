<template>
  <div class="card">
  <table>
    <div>Nome do Evento: {{ nome }}</div>
    <div>Local do Evento: {{ local }}</div>
    <div>Data do Evento: {{ data }}</div>
    <h2>Capacidade total:{{ capacidade }}</h2>
    <button v-if="mostraBotao" @click="emit('excluir')">
      Excluir
    </button>
    <button v-if="mostraBotao" @click="emit('editar')">
  Editar
</button>
  </table>
  </div>
</template>

<script setup>
const props = defineProps({
  nome: {
    type: String,
    default: '',
  },
  local: {
    type: String,
    default: '',
  },
  data: {
    type: String,
    default: '',
  },
  capacidade: {
    type: Number,
    default: 0,
  },
  mostraBotao: {
    type: Boolean,
    default: false,
  },
  
});

const emit = defineEmits(['excluir', 'editar']);
</script>

<style scoped>
.card {
  border: 2px solid black;
  border-radius: 20px;
  padding: 10px;
  margin: 10px;
  color: white;
}
</style>