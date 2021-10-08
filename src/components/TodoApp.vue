<template>
  <div class="container">
    <h2 class="text-center mt-5">ToDo App</h2>
    <div class="d-flex">
      <input
        v-model="task"
        @keyup.enter="createTask"
        type="text"
        placeholder="Enter task"
        class="form-control"
      />
      <button @click="createTask" class="btn btn-success rounded-0">
        Create
      </button>
      <select v-model="index" @change="changeOption">
        <option disabled value="">Choose from list</option>
        <option
          v-for="option in options"
          :key="option.index"
          :value="option.index"
        >
          {{ option.title }}
        </option>
      </select>
    </div>

    <table class="table table-bordered table-secondary mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === 'finished' }"
              >{{ task.title }}
            </span>
          </td>
          <td style="width: 130px">
            <span
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'finished',
              }"
              class="pointer"
              >{{ upperCase(task.status) }}</span
            >
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="removeTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  title: '',
  props: {
    msg: String,
  },
  title: {
    type: String,
  },
  options: {
    type: Array,
    default: () => [],
  },

  data() {
    return {
      task: '',
      editedTask: null,
      statuses: ['to-do', 'in-progress', 'finished'],
      selectedSort: '',
      sortOptions: [
        { value: 'title', name: 'By name' },
        { value: 'body', name: 'By content' },
      ],
      tasks: [
        {
          title: 'Go to the work',
          status: 'to-do',
        },
        {
          title: 'Meeting with friends at 13:00',
          status: 'in-progress',
        },
        {
          title: 'Go to the shop at night',
          status: 'finished',
        },
      ],
    }
  },
  methods: {
    createTask() {
      if (this.task.length === 0) return
      if (this.editedTask === null) {
        this.tasks.push({
          title: this.task,
          status: 'to-do',
        })
        this.task = ''
      } else {
        this.tasks[this.editedTask].title = this.task
        this.editedTask = null
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
    },
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status)
      if (++newIndex > 2) newIndex = 0
      this.tasks[index].status = this.statuses[newIndex]
    },
    editTask(index) {
      this.task = this.tasks[index].title
      this.editedTask = index
    },
    upperCase(str) {
      return str.toUpperCase()
    },
    changeOption(event) {
      this.$emit('on-select', event.target.value)
    },
  },
}
</script>

<style scoped>
* {
  text-transform: uppercase;
}
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
