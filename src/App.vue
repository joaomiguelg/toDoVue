<script setup>
import { reactive, registerRuntimeCompiler } from 'vue';

const state = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar Es6',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true,
      }
    ]
})

const getTarefasPendentes = () => {
   return state.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
   return state.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {

  const {filtro} = state;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
       return getTarefasFinalizadas();

       default:
        return state.tarefas;
  }

}

const cadastraTarefa = () => {

  const tarefaNova = {
    titulo: state.tarefaTemp,
    finalizada: false,

  }

  state.tarefas.push(tarefaNova);
  state.tarefaTemp = '';

}

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas Tarefas</h1>
      <p>
        Voce possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="state.tarefaTemp" @change="evento => state.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => state.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas Tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
          {{tarefa.titulo}}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>

  .done {
    text-decoration: line-through;
  }

</style>
