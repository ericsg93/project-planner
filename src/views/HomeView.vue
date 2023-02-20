<template>
  <div class="home">
    <div v-if="projects.length">
      <FilterNav @filter="current = $event" :current="current" />
      <div v-for="project in this.filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import FilterNav from "@/components/FilterNav.vue";
import SingleProject from "@/components/SingleProject.vue";
// @ is an alias to /src

export default {
  name: "HomeView",
  components: {
    FilterNav,
    SingleProject,
  },
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  computed: {
    filteredProjects() {
      if (this.current === "completed") {
        return this.projects.filter((x) => x.complete);
      }

      if (this.current === "ongoing") {
        return this.projects.filter((x) => !x.complete);
      }

      return this.projects;
    },
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((p) => p.id !== id);
    },
    handleComplete(id) {
      let project = this.projects.find((p) => p.id === id);
      project.complete = !project.complete;
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
};
</script>
