<template>
  <div class="container grid-xs py-2">
    <!-- <img class="img-responsive img-logo" src="@/assets/logo-jc.png" alt="Logo JC" /> -->

    <form @submit.prevent="add(todo)">
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

import { mapActions, mapState } from 'vuex'

export default {
  name: 'App',
  components: { Todo },
  data() {
    return { todo: { checked: false }}
  },
  computed: {
    ...mapState(['todos', 'loading'])
  },
  methods: {
    ...mapActions(['addTodo', 'toggleTodo', 'removeTodo']),

    async add(todo){
      await this.addTodo(todo)
      this.todo = { checked: false }
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