<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue'
import Formulario from './components/Formulario.vue'
import ListaDeTarefas from './components/ListaDeTarefas.vue'

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas:[
      {
        titulo: 'Estudar ES6',
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

  const pegarTarefasPendentes = () => {
    // Esse sinal ! significa negação //
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const pegarTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const pegarTarefasFiltradas = () => {
    const {filtro} = estado;

    switch(filtro){
      case 'pendentes': 
        return pegarTarefasPendentes();
      case 'finalizadas':
        return pegarTarefasFinalizadas();
      default:
        return estado.tarefas; 
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    // Para limpar o campo do input //
    estado.tarefaTemp = '';
  }
</script>

<template>

  <div class="container">
                      <!-- COMPONENTES CRIADOS -->
      <Cabecalho :tarefas-pendentes="pegarTarefasPendentes().length"/>
      <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :editar-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" />
      <ListaDeTarefas :tarefas="pegarTarefasFiltradas()" />
  </div>
</template>


