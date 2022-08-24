<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="tasks.length">
      <div v-for="task in filteredProjects" :key="task.id">
        <SingleTask
          :task="task"
          @delete="handleDelete"
          @completed="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleTask from "../components/SingleTask.vue";
import FilterNav from "../components/FilterNav.vue";

export default {
  name: "HomeView",
  components: { SingleTask, FilterNav },
  data() {
    return {
      tasks: [],
      current: "all",
    };
  },
  mounted() {
    fetch("http://localhost:3000/tasks")
      .then((res) => res.json())
      .then((data) => (this.tasks = data))
      .catch((err) => console.error(err));
  },
  methods: {
    handleDelete(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    handleComplete(id) {
      let task = this.tasks.find((task) => task.id === id);
      task.completed = !task.completed;
    },
  },
  computed: {
    filteredProjects() {
      switch (this.current) {
        case "completed":
          return this.tasks.filter((task) => task.completed);
        case "ongoing":
          return this.tasks.filter((task) => !task.completed);
        default:
          return this.tasks;
      }
    },
  },
};
</script>
