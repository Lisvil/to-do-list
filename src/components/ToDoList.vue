<template>
  <div class="container">
    <h3>To do list</h3>
    <div class="add-task__container">
      <div class="add-task">
        <input class="add-task__input" :class="{input__error: error}" placeholder="Add new task"  v-model="task.title" @keyup.enter="addTask"/>
        <button @click="handleCancel">Cancel</button>
      </div>
      <p v-if="error" class="add-task__error"> Fill the field</p>
    </div>
    <div class="tasks__container">
      <TaskList :tasks="newTasks"  @edit-task="onEditTask" @finish-task="onFinishTask"/>
      <TaskList :tasks="finishedTasks" :finishedList="true" @delete="onDeleteTask"/>
    </div>
</div>

</template>
<script>
import TaskList from "@/components/TaskList.vue";
export default {
  name: 'ToDoList',
  components: {TaskList},
  data() {
    return {
      task: {id: 0, title: '', done: false},
      error: false,
      newTasks: [
        {id: 1, title: 'First task', done: false},
        {id: 2, title: 'Test task', done: false},
        {id: 3, title: 'Test task with long title. Watch how to export svg to a component', done: false}
      ],
      finishedTasks: [],
    }
  },
  methods: {
    addTask() {
      if (!this.task.id) {
        if (this.task.title) {
          this.error = false
          this.task.id = Math.floor(Math.random() * 1000) + 1
          this.newTasks.push({...this.task})
          this.resetTask()
        } else {
          this.error = true
        }

        return
      } 

      if (this.task.title) {
        this.error = false
        this.newTasks[this.newTasks.findIndex(f => f.id === this.task.id)] = {...this.task}
        this.resetTask()
      } else 
        this.error = true
    },
    onEditTask(e) {
      this.task = e
    },
    onFinishTask(e) {
      this.newTasks.splice(this.newTasks.findIndex(f => f.id === e.id), 1)
      this.finishedTasks.push(e)
    },
    onDeleteTask(e) {
      this.finishedTasks.splice(this.finishedTasks.findIndex(f => f.id === e.id), 1)
    },
    handleCancel() {
      this.resetTask()
    },
    resetTask() {
      this.task = {id: 0, title: '', done: false}
    }
  }
}
</script>
<style lang="scss" scoped>
.container {
  padding: 10px;
  h3 {
    margin: 20px;
    text-align: center;
  }
  .add-task__container {
    width: 80%;
    margin: 0 auto;
    padding: 0 10%;
    .add-task__error {
      font-size: 14px;
      color: rgb(255, 92, 92);
      margin: 0 auto;
      margin-top: 8px;
      padding-left: 2%;
      width: 80%;
    }
    .add-task {
      display: flex;
      align-items: center;
      justify-content: center;
      input {
        padding: 10px 10px;
        outline: none;
        border: 0;
        border-radius: 5px;
        box-shadow: 0 0 5px #5e5e5e3c;
        background-color: #eaeaeac7;
        border: 1px solid rgba(218, 218, 218, 0.479);
        width: 70%;
        display: block;
        color: #4d4d4d;
        font-size: 14px;
        &::placeholder {
          color: #7a7a7a;
        }
      }
      .input__error {
        border-color: red;
      }
      button {
        display: block;
        margin-left: 20px;
        cursor: pointer;
        padding: 7px 10px;
        font-size: 14px;
        background-color: #667ecd;
        outline: none;
        border: 0;
        color: #FFFFFF;
        border-radius: 5px;
        transition: all 0.3s ease;
        &:hover {
          background: darken( #667ecd, 10);
        }
      }
    }
  }
  .tasks__container {
    display: flex;
    padding: 20px 0;
    width: 80%;
    margin: 0 auto;
    justify-content: space-between;
    align-items: flex-start;
    @media(max-width: 700px) {
      flex-direction: column;
    }
  }
}
</style>
