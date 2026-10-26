<template>
  <div class="container mt-5">
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

        <ul class="list-group mt-3">
          <task-simple
            v-for="task in tasks"
            :key="task.id"
            :task="task"
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
      tasks: [],
    };
  },
  methods: {
    addTask() {
      const task = {
        id: crypto.randomUUID(),
        taskName: this.taskName,
      };

      this.tasks.push(task);
      this.taskName = "";
      this.$refs.addTaskInput.focus();
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
