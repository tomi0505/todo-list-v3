<template>
  <div class="container my-5">
    <div class="row justify-content-center">
      <div class="col-6">
        <div class="input-group">
          <input
            type="text"
            class="form-control"
            placeholder="Wprowadź zadanie na dziś..."
            aria-label="Example text with button addon"
            aria-describedby="addTaskBtn"
            v-model="taskName"
            ref="addTaskInput"
            @keyup.enter="addTask"
          />
          <button
            class="btn btn-primary"
            type="button"
            id="addTaskBtn"
            @click="addTask"
          >
            Dodaj
          </button>
        </div>

        <div class="mt-1 small text-danger" v-show="showTaskNameError">
          Wypełnij powyższe pole.
        </div>

        <ul class="list-group mt-3" v-if="tasks.length">
          <task-simple
            v-for="task in tasks"
            :key="task.id"
            :task="task"
            :remove-task="removeTask"
          ></task-simple>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import TaskSimple from "./components/TaskSimple.vue";

export default {
  components: {
    TaskSimple,
  },
  data() {
    return {
      taskName: "",
      showTaskNameError: false,
      tasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.taskName.trim() !== "") {
        const task = {
          id: crypto.randomUUID(),
          taskName: this.taskName,
        };

        this.tasks.push(task);
        this.showTaskNameError = false;
        this.taskName = "";
        this.$refs.addTaskInput.focus();
      } else {
        this.showTaskNameError = true;
      }
    },
    removeTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },
  mounted() {
    this.$refs.addTaskInput.focus();
  },
};
</script>

<style lang="scss">
body {
  background-color: #5f9ea0 !important;
}
</style>
