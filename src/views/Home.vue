<template>
  <div class="container grid-xs py-2">
    <!-- <img class="img-responsive img-logo" src="@/assets/logo-jc.png" alt="Logo JC" /> -->

    <form @submit.prevent="addTodo(todo)">
      <div class="input-group">
        <input type="text" class="form-input" v-model="todo.description" placeholder="Novo todo" />
        <button class="btn btn-primary input-group-btn" :class="{loading}">Add</button>
      </div>
    </form>
    <div class="todo-list">
      <todo v-for="t in todos" :key="t.id" @toggle="toggleTodo" @remove="removeTodo" :todo="t" />
    </div>
  </div>
</template>

<script>
import Todo from "@/components/Todo"

export default {
  name: 'App',
  components: { Todo },
  data() {
    return { todo: { checked: false }}
  },
  computed: {
    todos() {
      return this.$store.state.todos
    },
    loading() {
      return this.$store.state.loading
    }
  },
  methods: {
    async addTodo(todo){
      await this.$store.dispatch("addTodo", todo)
      this.todo = { checked: false }
    },
    toggleTodo(todo) {
      this.$store.dispatch('toggleTodo', todo)
    },
    removeTodo(todo) {
      this.$store.dispatch('removeTodo', todo)
    }
  }
}
</script>

<style scoped>
/* .img-logo {
  max-width: 150px;
  margin: 0 auto;
  padding: 25px;
} */
.todo-list {
  padding-top: 2rem;
}
</style>