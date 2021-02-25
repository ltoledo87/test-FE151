<template>
  <b-navbar toggleable="lg" type="dark" variant="primary">
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item><router-link to="/">Home</router-link></b-nav-item>
        <b-nav-item
          ><router-link to="/cursos"
            >Cursos Disponibles</router-link
          ></b-nav-item
        >>
      </b-navbar-nav>
    </b-collapse>
    <b-navbar-brand><router-link to="/"> Mi App </router-link></b-navbar-brand>
  </b-navbar>
</template>

<script>
export default {
  name: "Navbar",
  computed: {},
};
</script>

<style scope>
</style>