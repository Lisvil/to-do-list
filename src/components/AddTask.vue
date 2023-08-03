<template>
  <div class="add-task">
    <input class="add-task__input" placeholder="Add new task"  v-model="task.title" @keyup.enter="addTask"/>
    <button v-if="Object.keys(this.editedTask).length" @click="handleCancel">Cancel</button>
  </div>
</template>

<script>
export default {
  name: 'AddTask',
  props: {editedTask: {type: Object, default: {}}},
  data() {
    return {
      task: {id: 0, title: '', done: false},
    }
  },
  watch: {
    editedTask: {
      handler(newValue, oldValue) {
        console.log('sadasd')
        this.task = {...this.editedTask}
      },
      deep: true
    }
  },
  methods: {
    addTask() {
      if (!Object.keys(this.editedTask).length) {
        this.task.id = Math.floor(Math.random() * 1000) + 1
        this.$emit('add-task', {...this.task})
        this.task.title = ''
      } else {
        console.log('edit in input')
        this.$emit('edit-task', this.task)
        this.task = {id: 0, title: '', done: false}
      }
    },
    handleCancel() {
      this.title = {id: 0, title: '', done: false}
      this.$emit('cancel')
    }
  }
}
</script>