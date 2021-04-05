<template>
  <div id="app">
    <div class="h-100 w-full flex items-center justify-center font-sans">
      <div
        class="bg-white rounded shadow-lg p-6 m-4 w-full lg:w-3/4 lg:max-w-lg"
      >
        <div class="mb-4">
          <h1 class="text-grey-darkest">The Next Billion Dollar ToDo App 😎</h1>

          <form class="inline-block flex mt-4" @submit.prevent>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
              placeholder="Add Todo"
              v-model="todoInput"
            />
            <button
              type="submit"
              v-on:click="addTodo"
              class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal"
            >
              Add
            </button>
          </form>
        </div>
        <div>
          <div
            class="flex mb-4 items-center"
            v-for="(todo, index) in todos"
            :key="index"
          >
            <p
              class="w-full"
              v-bind:class="{
                'text-grey-darkest': !todo.complete,
                'line-through text-green': todo.complete,
              }"
            >
              {{ todo.msg }}
            </p>
            <button
              v-if="!todo.complete"
              class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded hover:text-white text-green border-green hover:bg-green"
              @click="changeTodoState(index, true)"
            >
              Done
            </button>
            <button
              v-if="todo.complete"
              class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded hover:text-white text-grey border-grey hover:bg-grey"
              @click="changeTodoState(index, false)"
            >
              Not Done
            </button>

            <button
              class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red"
              @click="removeTodo(index)"
            >
              Remove
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

const LocalStorageKey = "VueToDoApp";

export default {
  name: "App",
  components: {},
  data() {
    return {
      todoInput: "",
      todos: [],
    };
  },
  methods: {
    addTodo: function () {
      if (this.todoInput == "") return;
      this.todos.push({ msg: this.todoInput, complete: false });
      this.todoInput = "";
      this.saveToLocalStorage();
    },
    changeTodoState: function (index, val) {
      this.todos[index].complete = val;
      this.saveToLocalStorage();
    },
    removeTodo: function (index) {
      if (index > -1) {
        this.todos.splice(index, 1);
        this.saveToLocalStorage();
      }
    },
    saveToLocalStorage: function () {
      localStorage.setItem(LocalStorageKey, JSON.stringify(this.todos));
    },
    getFromLocalStorage: function () {
      const dataFromLocalStorage = localStorage.getItem(LocalStorageKey);
      if (!dataFromLocalStorage) return;
      this.todos = JSON.parse(dataFromLocalStorage);
    },
  },
  mounted() {
    this.getFromLocalStorage();
  },
};
</script>

<style>
</style>
