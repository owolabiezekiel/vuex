<template>
  <div>
    <h3>Todos</h3>
    <div class="legend">
      <span>Double click to mark as complete</span>
      <span>
        <span class="incomplete-box"></span> = Incomplete
      </span>
      <span>
        <span class="complete-box"></span> = Complete
      </span>
    </div>
    <div class="todos">
      <div
        @dblclick="onDclClick(todo)"
        v-for="todo in allTodos"
        :key="todo.id"
        class="todo"
        v-bind:class="{'is-complete':todo.completed}"
      >
        {{ todo.title }}
        <i @click="deleteTodo(todo.id)" class="fas fa-trash-alt"></i>
        <em @click="showModal(todo)" class="far fa-edit"></em>
      </div>
      <Modal
        v-show="isModalVisible"
        @close="closeModal"
      >
      <input slot="title" type="text" v-model="todo_name">
      <button slot="updateButton"
          type="button"
          class="btn-green"
          aria-label="Close modal"
          @click="updateTodoTitle()"
        >
        Update
      </button>
      </Modal>
      </div>
  </div>
</template>

<script>
import Modal from "./Modal"
import { mapGetters, mapActions } from "vuex";

export default {
  name: "Todos",
  methods: {
    ...mapActions(["fetchTodos", "deleteTodo", "updateTodo"]),
    onDblClick(todo) {
      const updTodo = {
        id: todo.id,
        title: todo.title,
        completed: !todo.completed
      };

      this.updateTodo(updTodo);
    },
    onSnClick(){
    },
    showModal(todo) {
      this.currentTodo = todo
      this.todo_name = todo.title
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    updateTodoTitle(){
      const updTodo = {
        id: this.currentTodo.id,
        title: this.todo_name,
        completed: this.currentTodo.completed
      };
      this.updateTodo(updTodo);
      this.closeModal()
    }
  },
  computed: mapGetters(["allTodos"]),
  created() {
    this.fetchTodos();
  },
  data(){
    return{
      todo_name: "",
      currentTodo: null,
      isModalVisible: false,
    }
  },
  components:{
    Modal,
  }
};
</script>

<style scoped>
.todos {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 1rem;
}

.todo {
  border: 1px solid #ccc;
  background: #41b883;
  padding: 1rem;
  border-radius: 5px;
  text-align: center;
  position: relative;
  cursor: pointer;
}

i {
  position: absolute;
  bottom: 10px;
  right: 10px;
  color: #fff;
  cursor: pointer;
}

.legend {
  display: flex;
  justify-content: space-around;
  margin-bottom: 1rem;
}

.complete-box {
  display: inline-block;
  width: 10px;
  height: 10px;
  background: #35495e;
}

.incomplete-box {
  display: inline-block;
  width: 10px;
  height: 10px;
  background: #41b883;
}

.is-complete {
  background: #35495e;
  color: #fff;
}

@media (max-width: 500px) {
  .todos {
    grid-template-columns: 1fr;
  }
}
</style>
