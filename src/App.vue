<template>
  <div id="app">
    <todo-app></todo-app>
    <selected
      :index="selectedSort"
      :options="sortOptions"
      @on-select="onSelect"
    >
    </selected>
  </div>
</template>

<script>
import TodoApp from '@/components/TodoApp.vue'
export default {
  name: 'App',
  components: {
    TodoApp,
  },
  methods: {
    onSelect(value) {
      this.selectedSort = value
    },
    computed: {
      sortedPosts() {
        if (!this.selectedSort) {
          return this.tasks
        } else if (this.selectedSort === 'title') {
          return this.tasks.sort((a, b) => a.title.localeCompare(b.title))
        } else if (this.selectedSort === 'selected') {
          return this.status.sort((a, b) => a.status.localeCompare(b.status))
        }
      },
    },
  },
}
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
}
</style>
