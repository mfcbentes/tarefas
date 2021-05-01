<template>
  <div id="task">
    <h1>Componente Task</h1>
    <form v-on:submit.prevent="addTask">
      <input type="text" placeholder="Tarefa de hoje?" v-model="tarefa" />
      <button type="submit">Adicionar</button>
    </form>
    <Item v-bind:lista="tarefas" />
  </div>
</template>
<script>
import Item from "./Item";

export default {
  name: "Task",
  data() {
    return {
      tarefa: "",
      tarefas: [],
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
  },
  components: {
    Item,
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
</style>