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

        <!-- Tabs start -->
        <div class="mt-4" v-if="tasks.length || removedTasks.length">
          <ul class="nav nav-tabs" id="Tabs" role="tablist">
            <li class="nav-item" role="presentation" v-if="tasks.length">
              <button
                class="nav-link"
                id="tab1-tab"
                data-bs-toggle="tab"
                data-bs-target="#tab1"
                type="button"
                role="tab"
                aria-controls="tab1"
                aria-selected="true"
                :class="{ active: tab === 1 }"
                @click="tab = 1"
              >
                Aktywne
              </button>
            </li>
            <li class="nav-item" role="presentation" v-if="removedTasks.length">
              <button
                class="nav-link"
                id="tab2-tab"
                data-bs-toggle="tab"
                data-bs-target="#tab2"
                type="button"
                role="tab"
                aria-controls="tab2"
                aria-selected="false"
                :class="{ active: tab === 2 }"
                @click="tab = 2"
              >
                Zakończone
              </button>
            </li>
          </ul>

          <div id="TabsContent">
            <div
              id="tab1"
              role="tabpanel"
              aria-labelledby="tab1-tab"
              v-if="tasks.length"
            >
              <ul class="list-group mt-2" v-show="tab === 1">
                <task-simple
                  v-for="task in tasks"
                  :key="task.id"
                  :task="task"
                  :remove-task="removeTask"
                ></task-simple>
              </ul>
            </div>
            <div
              id="tab2"
              role="tabpanel"
              aria-labelledby="tab2-tab"
              v-if="removedTasks.length"
            >
              <ul class="list-group mt-2" v-show="tab === 2">
                <task-simple
                  v-for="task in removedTasks"
                  :key="task.id"
                  :task="task"
                  :remove-task="removeTaskPermanently"
                ></task-simple>
              </ul>
            </div>
          </div>
        </div>
        <!-- Tabs end -->
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
      tab: 1,
      taskName: "",
      showTaskNameError: false,
      tasks: [],
      removedTasks: [],
    };
  },
  watch: {
    tasks(newTasks) {
      if (newTasks.length === 0 && this.removedTasks.length > 0) {
        this.tab = 2;
      }
    },
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
        this.tab = 1;
        this.$refs.addTaskInput.focus();
      } else {
        this.showTaskNameError = true;
      }
    },
    removeTask(id) {
      const removedTask = this.tasks.filter((task) => task.id === id)[0];
      this.tasks = this.tasks.filter((task) => task.id !== id);
      this.removedTasks.push(removedTask);
    },
    removeTaskPermanently(id) {
      console.log("removeTaskPermanently(): ", id);
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
