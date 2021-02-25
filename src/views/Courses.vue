<template>
  <div class="home">
    <Navbar />
    <Course />
    <Loader v-if="loading" />
  </div>
</template>

<script>
import Loader from "@/components/Loader.vue";
import Navbar from "@/components/Navbar.vue";
import Course from "@/components/Course.vue";

export default {
  name: "Cursos",
  components: {
    Loader,
    Course,
    Navbar,
  },
  data: () => ({
    loading: true,
  }),
  mounted() {
    setTimeout(() => {
      this.loading = false;
    }, 3000);
  },
};
</script>
