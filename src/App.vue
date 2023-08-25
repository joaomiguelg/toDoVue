<script setup>
import { reactive, registerRuntimeCompiler } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import List from './components/List.vue';

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
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => state.filtro = evento.target.value" :tarefa-temp="state.tarefaTemp" :edita-tarefa-temp="evento => state.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" />
    <List :tarefas="getTarefasFiltradas()" />
  </div>
</template>


