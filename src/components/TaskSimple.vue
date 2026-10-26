<template>
  <li class="list-group-item">{{ task.taskName }}</li>
</template>

<script>
export default {
  props: {
    task: Object,
  },
};
</script>
