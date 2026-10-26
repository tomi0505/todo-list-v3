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

        <div class="my-tabs" v-if="tasks.length > 0 || removedTasks.length > 0">
          <div class="mt-3 my-tabs__tabs-container">
            <div
              id="activeTasksTab"
              class="text-white my-tab-simple-b"
              :class="{
                'active-b': activeTab === 1,
                'd-none-b': tasks.length < 1,
              }"
              @click="active1Tab"
            >
              Aktywne
            </div>
            <div
              id="removedTasksTab"
              class="text-white my-tab-simple-b"
              :class="{
                'active-b': activeTab === 2,
                'd-none-b': removedTasks.length < 1,
              }"
              @click="active2Tab"
            >
              Zakończone
            </div>
          </div>

          <div>
            <div id="activeTasksContent" v-show="activeTab === 1">
              <ul class="list-group mt-2">
                <task-simple
                  v-for="task in tasks"
                  :key="task.id"
                  :task="task"
                  :remove-task="removeTask"
                ></task-simple>
              </ul>
            </div>

            <div class="removedTasksContent" v-show="activeTab === 2">
              <ul class="list-group mt-2">
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
      activeTab: 1,
      taskName: "",
      showTaskNameError: false,
      tasks: [],
      removedTasks: [],
    };
  },
  watch: {
    tasks: {
      handler(value) {
        if (value.length < 1 && this.removedTasks.length > 0) {
          this.activeTab = 2;
        } else if (value.length > 0) {
          this.activeTab = 1;
        }
      },
      deep: true,
    },
    removedTasks: {
      handler(value) {
        if (value.length < 1 && this.tasks.length > 0) {
          this.activeTab = 1;
        } else if (value.length > 0) {
          this.activeTab = 2;
        }
      },
      deep: true,
    },
  },
  methods: {
    active1Tab() {
      this.activeTab = 1;
    },
    active2Tab() {
      this.activeTab = 2;
    },
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
      const removedTask = this.tasks.filter((task) => task.id === id)[0];
      this.tasks = this.tasks.filter((task) => task.id !== id);
      this.removedTasks.push(removedTask);
    },
    removeTaskPermanently(id) {
      this.removedTasks = this.removedTasks.filter((task) => task.id !== id);
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

.d-none-b {
  display: none;
}

.my-tabs {
  &__tabs-container {
    display: flex;
    gap: 8px;
  }
}

.my-tab-simple-b {
  padding: 6px 12px;
  border-radius: 0.375rem;
  cursor: pointer;
  background-color: rgb(13, 110, 253);

  &:hover {
    background-color: rgb(11, 94, 215);
  }
}

.active-b {
  display: block;
  background-color: rgb(11, 94, 215);
}
</style>
