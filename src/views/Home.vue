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
    return { todo: { checked: false }, loading: false}
  },
  computed: {
    todos() {
      return this.$store.state.todos
    }
  },
  methods: {
    async addTodo(todo){
      try {
        this.loading = true
        await this.$store.dispatch("addTodo", todo)
        this.todo = { checked: false }
      } finally {
        this.loading = false
      }
    },
    toggleTodo(todo) {
      const index = this.todos.findIndex(item => item.id === todo.id)
      if (index > -1) {
        const checked = !this.todos[index].checked
        this.$set(this.todos, index, { ...this.todos[index], checked })
      }
    },
    removeTodo(todo) {
      const index = this.todos.findIndex(item => item.id === todo.id)
      if (index > -1) {
        this.$delete(this.todos, index)
      }
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