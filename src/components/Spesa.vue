<template>
  <div class="container">
    <h1 class="text-center">Lista della spesa down</h1>

    <form @submit.prevent="addTodo">
      <div class="form-group">
        <label for="todo">Cosa devo comprare?</label>
        <input v-model="todo" type="text" class="form-control" id="todo" />
        <small id="todoHelp" class="form-text text-muted"
          >Inserisci qui quello che ti manca</small
        >
      </div>
      <button type="submit" class="btn btn-primary">
        Aggiungi alla lista della spesa
      </button>
    </form>

    <div class="lista">
      <ul class="list-group">
        <div v-for="(item, index) in todos" :key="index">
          <li
            class="list-group-item d-flex justify-content-between align-items-center"
          >
            <span
              :class="{
                line: item.isDone,
              }"
              >{{ item.title }}</span
            >
            <div class="d-flex justify-content-end align-items-center">
              <button
                v-if="!item.isDone"
                @click="markDone(item)"
                type="button"
                class="btn btn-success mr-1"
              >
                Fatto
              </button>
              <button
                @click="markDelete(index)"
                type="button"
                class="btn btn-danger"
              >
                Cancella
              </button>
            </div>
          </li>
        </div>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Spesa",

  data: () => ({
    todo: "",
    todos: [],
  }),

  watch: {
    todos: {
      handler() {
        localStorage.todos = JSON.stringify(this.todos);
      },
      deep: true,
    },
  },

  methods: {
    addTodo() {
      this.todos.push({
        title: this.todo,
        isDone: false,
      });
      this.todo = "";
    },
    markDelete(ind) {
      this.todos.splice(ind, 1);
    },
    markDone(item) {
      item.isDone = true;
    },
  },
  mounted() {
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.todos);
    }
  },
};
</script>

<style scoped>
.lista {
  margin-top: 2rem;
}

.line {
  text-decoration: line-through;
}

h1 {
  margin-top: 2rem;
}
</style>
