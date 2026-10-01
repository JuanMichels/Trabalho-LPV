<template>
  <h1>Eventos</h1>
  <div>
    <form id="formulario">
      <label for="nome">Nome do evento: </label>
      <input type="text" id="nome" name="nome" v-model="novoEvento.nome" />
      <br>
      <label for="local">Local do evento: </label>
      <input type="text" id="local" name="local" v-model="novoEvento.local" />
      <br>
      <label for="data">Data do evento: </label>
      <input type="text" id="data" name="data" v-model="novoEvento.data" />
      <br>
      <label for="capacidade">Capacidade do evento: </label>
      <input type="number" id="capacidade" name="capacidade" v-model="novoEvento.capacidade" />
      <br>
      <button type="button" @click="adicionarEvento">Adicionar Evento</button>
      <button type="button" @click="salvarEvento(salvarEvento)">Salvar</button>
    </form>
  </div>
  <div class="lista">
    <div v-for="evento in eventos" :key="evento.nome" class="card">
      <Card :nome="evento.nome" :local="evento.local" :data="evento.data" :capacidade="evento.capacidade" mostraBotao
        @excluir="excluirEvento(evento)" @editar="editarEvento(evento)"></Card>
    </div>
  </div>
</template>

<script setup>
import Card from './components/cards.vue';
import { onMounted, pushScopeId, ref } from 'vue';
import axios from 'axios'; ''

const novoEvento = ref({
  nome: '',
  local: '',
  data: '',
  capacidade: 0
})

async function adicionarEvento() {
  await axios.post('https://api-lpv.onrender.com/eventos/', {
    nome: novoEvento.value.nome,
    local: novoEvento.value.local,
    data: novoEvento.value.data,
    capacidade: novoEvento.value.capacidade
  })

  novoEvento.value = {
    nome: '',
    local: '',
    data: '',
    capacidade: 0
  }
  await obterDados();
}
const eventos = ref([

]);


onMounted(() => {
  obterDados();
});

async function obterDados() {
  const resposta = await axios.get('https://api-lpv.onrender.com/eventos/');
  eventos.value = resposta.data.data;

}

async function excluirEvento(evento) {
  console.log(`Excluindo evento: `, evento);
  await axios.delete(`https://api-lpv.onrender.com/eventos/${evento.id}`);
  await obterDados();
}
const idEditar = ref(null);
async function editarEvento(evento) {
  idEditar.value = evento.id;
  console.log(`Editando evento: `, evento);
  await obterDados();
  novoEvento.value = {
    nome: evento.nome,
    local: evento.local,
    data: evento.data,
    capacidade: evento.capacidade
  }
}

async function salvarEvento(evento) {
  console.log(`Salvando evento: `, novoEvento.value);
  await axios.patch(`https://api-lpv.onrender.com/eventos/${idEditar.value}`, {
    nome: novoEvento.value.nome,
    local: novoEvento.value.local,
    data: novoEvento.value.data,
    capacidade: novoEvento.value.capacidade
  })
  await obterDados();
  novoEvento.value = {
    nome: '',
    local: '',
    data: '',
    capacidade: 0
  }
}

</script>

<style scoped>
.lista {
  height: 200px;
  width: 95%;
  margin: 10px;
  display: flex;
}

.card {
  width: 80%;
}
</style>