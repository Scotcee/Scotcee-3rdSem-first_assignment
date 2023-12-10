<template>
  <div class="container" style="max-width: 600px">
    <h2 class="text-center mt-3 text-white">My Vue ToDo App</h2>
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="w-100 form-control"
      />
    </div>
    <table class="table table-bordered">
      <thead class="thead-dark">
        <tr>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td class="table-success">
            <div class="text-center" @click="editTask(index)">
              <span style="cursor: pointer">
                <i class="fas fa-pencil-alt"></i>
              </span>
            </div>
          </td>
          <td
            class="table-success"
            :class="{ 'line-through': task.status === 'done' }"
          >
            {{ task.name }}
          </td>
          <td class="table-success">
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-primary': task.status === 'in-progress',
                'text-success': task.status === 'done',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>

          <td class="table-danger">
            <div class="text-center" @click="deleteTask(index)">
              <span style="cursor: pointer">
                <i class="fas fa-trash-alt"></i>
              </span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="mx-auto">
      <button
        type="button"
        class="btn btn-success rounded-5 px-3 py-1"
        @click="submitTask"
      >
        SUBMIT
      </button>
    </div>
    <!-- Popup Message -->
    <div v-if="showPopup" class="popup-message">
      {{ popupMessage }}
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "done"],
      tasks: [
        {
          name: "Eat breakfast",
          status: "to-do",
        },
        {
          name: "Eat lunch",
          status: "in-progress",
        },
        {
          name: "Eat dinner",
          status: "done",
        },
      ],
      showPopup: false,
      popupMessage: "",
    };
  },
  methods: {
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    submitTask() {
      if (this.task.trim() === "") {
        this.showPopupMessage("Task cannot be empty!");
        return;
      }

      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.capitalizeFirstChar(this.task);
        this.editedTask = null;
      } else {
        this.tasks.push({
          name: this.capitalizeFirstChar(this.task),
          status: "to-do",
        });
      }

      this.task = "";
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    showPopupMessage(message) {
      this.showPopup = true;
      this.popupMessage = message;

      setTimeout(() => {
        this.hidePopupMessage();
      }, 2000); // Hide popup after 2 seconds
    },
    hidePopupMessage() {
      this.showPopup = false;
      this.popupMessage = "";
    },
  },
};
</script>

<style scoped>
.noselect {
  user-select: none;
}
.line-through {
  text-decoration: line-through;
}
.popup-message {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: red;
  color: white;
  font-size: 15px;
  padding: 10px;
  border: 1px solid red;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  z-index: 9999;
}
</style>
