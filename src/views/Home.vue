<template>
  <div class="home">
    <Navbar />
    <Profile />
    <Loader v-if="loading" />
  </div>
</template>

<script>
import Loader from "@/components/Loader.vue";
import Navbar from "@/components/Navbar.vue";
import Profile from "@/components/Profile.vue";

export default {
  name: "Home",
  components: {
    Loader,
    Profile,
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
