<template>
  <div id="task">
    <form v-on:submit.prevent="addTask">
      <input type="text" placeholder="Tarefa de hoje?" v-model="tarefa" />
      <button type="submit">Adicionar</button>
    </form>
    <Item v-bind:lista="tarefas" v-bind:delete="deleteTask" />
    <span v-show="tarefas.length > 0">
      Você tem
      <strong v-bind:class="{ pend: pendente }">{{ tarefas.length }}</strong>
      tarefas pendentes.
    </span>
  </div>
</template>
<script>
import Item from "./Item";

export default {
  name: "Task",
  components: {
    Item,
  },
  data() {
    return {
      tarefa: "",
      tarefas: [],
      pendente: false,
    };
  },
  methods: {
    addTask() {
      if (this.tarefa !== "") {
        this.tarefas.push({
          text: this.tarefa,
          key: Date.now(),
        });
      } else {
        alert("Digite uma tarefa...");
        return;
      }
      this.tarefa = "";
    },
    deleteTask(key) {
      let filtro = this.tarefas.filter((item) => {
        return item.key !== key;
      });

      return (this.tarefas = filtro);
    },
  },
  watch: {
    tarefas: {
      deep: true,
      handler() {
        localStorage.setItem("tasks", JSON.stringify(this.tarefas));
        this.tarefas.length > 4
          ? (this.pendente = true)
          : (this.pendente = false);
      },
    },
  },
  created() {
    const minhaLista = localStorage.getItem("tasks");
    this.tarefas = JSON.parse(minhaLista) || [];
  },
};
</script>
<style scoped>
#task {
  max-width: 700px;
  background-color: #fff;
  border-radius: 4px;
  padding: 20px;
  margin: 20px auto;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
}

form {
  margin-top: 30px;
  display: flex;
  flex-direction: row;
}

button {
  cursor: pointer;
  background: #0f5959;
  border: 0;
  border-radius: 4px;
  margin-left: 10px;
  padding: 0 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
}

input {
  flex: 1;
  border: 1px solid #eee;
  padding: 6px 10px;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.pend {
  color: #ff0000;
}
</style>