<template>
  <li class="list-group-item d-flex justify-content-between align-items-center">
    <span>{{ task.taskName }}</span>
    <button class="btn btn-sm btn-secondary" @click="removeTask(task.id)">
      Zakończ
    </button>
  </li>
</template>

<script>
export default {
  props: {
    task: Object,
    removeTask: Function,
  },
};
</script>
