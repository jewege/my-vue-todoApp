<template lang="">
  <div class="container">
    <h2 class="text-center mt-5 title">My Vue Todo App</h2>

    <!-- input -->
    <div class="d-flex gap-2 mt-5">
      <input
        v-model="task"
        v-on:keydown.enter="submitTask"
        type="text"
        placeholder="enter task"
        class="form-control"
      />
      <button class="d-flex btn btn-warning rounded-1" @click="submitTask()">
        <i class="bi bi-plus"></i>
      </button>
    </div>
    <div v-if="this.tasks.length === 0" class="mt-3 text-center fst-italic">
      <table class="table table-bordered mt-3">
        <tbody>
          <tr>
            <td style="color: gray">Add Todolist</td>
          </tr>
        </tbody>
      </table>
    </div>
    <!-- task-table -->
    <table v-else class="table table-bordered mt-3">
      <thead>
        <tr>
          <th scope="col" class="text-center">Task</th>
          <th scope="col" class="text-center">Status</th>
          <th scope="col" class="text-center" style="width: 50px">Edit</th>
          <th scope="col" class="text-center" style="width: 50px">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td class="fw-bold">{{ task.name }}</td>
          <span
            class="text-center pointer fw-bold d-flex align-items-center"
            style="width: 120px"
            @click="changeStatus(index)"
            :class="{
              'text-danger': task.status === 'To-do',
              'text-warning': task.status === 'In-progress',
              'text-success': task.status === 'Finished',
            }"
            ><td>{{ task.status }}</td>
          </span>
          <td>
            <div class="text-center pointer text-primary" @click="editTask(index)">
              <i class="bi bi-pencil"></i>
            </div>
          </td>
          <td>
            <div class="text-center pointer text-danger" @click="deleteTask(index)">
              <i class="bi bi-trash"></i>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  name: "TodoApp",
  data() {
    return {
      task: "",
      editedTask: null,
      availabelStatuses: ["To-do", "In-progress", "Finished"],
      tasks: [],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: this.availabelStatuses[0],
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = "";
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    changeStatus(index) {
      let newIndex = this.availabelStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availabelStatuses[newIndex];
    },
  },
};
</script>
<style scoped>
.container {
  width: 60%;
}
.pointer {
  cursor: pointer;
}

.title {
  font-family: "Lobster", cursive;
}
</style>
