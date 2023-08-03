<template>
  <div class="tasks">
    <h3 class="tasks__title">{{status}} <span v-if="finishedList"> - ({{ tasks.length }})</span>
    </h3>
    <div class="tasks__list" :class="{finished_list: finishedList}">
      <div class="task" v-for="task in tasks" :key="task.id">
        <p class="task__text">{{task.title}}</p>
        <div class="task__buttons">
          <template v-if="!finishedList">
            <button class="task__button_edit" @click="editTask(task)">Edit</button>
            <button class="task__button_finish" @click="finishTask(task)">Finish </button>
          </template>
          <button v-else class="task__button_delete" @click="deleteTask(task)">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TaskList',
  props: {
    tasks: {type: Array, default: []},
    finishedList: {type: Boolean, default: false}
  },
  data() {
    return {
      status: this.finishedList ? 'Finished tasks' : 'New tasks'
    }
  },
  methods: {
    editTask(task) {
      this.$emit('edit-task', {...task})
    },
    finishTask(task) {
      this.$emit('finish-task', {...task})
    },
    deleteTask(task) {
      this.$emit('delete', {...task})
    }
  }
}
</script>

<style lang="scss">
.tasks {
  width: 45%;
  @media(max-width: 700px) {
    width: 100%;
  }
  .tasks__title {
    text-align: center;
    margin: 20px 0 10px 0;
  }
  .tasks__list {
    width: 100%;
    min-height: 200px;
    background-color: #e8edf6c3;
    border-radius: 5px;
    box-shadow: 0 0 7px #adadad9d;
    padding: 10px 15px;
    .task {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 7px;
      background: #fdfdfd;
      box-shadow: 0 0 10px #d0d0d07e;
      margin: 10px 0;
      border-radius: 5px;
      font-size: 14px;
      .task__buttons {
        display: flex;
        button {
          margin-left: 5px;
          padding: 5px 7px;
          cursor: pointer;
          border-radius: 4px;
          border: 0;
          color: #ffffff;
          font-size: 14px;  
          transition: all 0.3s ease;

        }
        .task__button_edit {
          background: #849dab;
          &:hover {
            background: darken(#849dab, 10);
          }
        }
        .task__button_finish {
          background: #6bd19e;
          &:hover {
            background: darken(#6bd19e, 10);
          }

        }
        .task__button_delete {
          background: #fa4454;
          &:hover {
            background: darken(#fa4454, 10);
          }

        }
      }
    }
  }
  .finished_list {
    background-color: #e8faf2d9;
  }
}
</style>