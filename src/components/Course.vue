<template>
  <b-container class="py-3">
    <b-card-group deck>
      <b-card
        v-for="course in dataCourses"
        v-bind:key="course.id"
        :title="course.attributes.name"
        :img-src="course.attributes.cover"
        img-width="288"
        img-height="200"
        img-alt="Image"
        img-top
      >
        <b-card-text>
          Dificultad: {{ course.attributes.difficulty }}
        </b-card-text>
        <b-card-text v-html="course.attributes.description"> </b-card-text>
        <template #footer>
          <b-button
            v-b-modal="'myModal-1'"
            variant="primary"
            v-on:click="fetchCourse(course.attributes.slug)"
            >Detalles</b-button
          >
        </template>
      </b-card>
    </b-card-group>
    <b-modal
      id="myModal-1"
      size="lg"
      title="Información del Curso"
      scrollable
      ok-only
    >
      <b-container v-if="!!course" class="text-center">
        <b-row class="py-3">
          <b-col>
            <b-avatar
              rounded
              :src="courseDetails.attributes.cover"
              size="15rem"
              variant="light"
            ></b-avatar
            ><br />
            <h2>{{ courseDetails.attributes.name }}</h2>
            Nivel de Dificultad: {{ courseDetails.attributes.difficulty }}
          </b-col>
        </b-row>
        <b-row class="py-3">
          <b-col v-html="courseDetails.attributes.description"> </b-col>
        </b-row>
        <b-row class="py-3">
          <b-col>
            <b-card header="Programa de Estudios" header-tag="header">
              <b-card-text>
                <b-row v-for="programa in dataProgram" v-bind:key="programa.id">
                  <b-col class="text-left">
                    <strong>{{ programa.attributes.name }}</strong>
                    <hr />
                  </b-col>
                </b-row>
              </b-card-text>
            </b-card>
          </b-col>
        </b-row>
      </b-container>
    </b-modal>
  </b-container>
</template>
<script>
export default {
  name: "Course",
  data() {
    return {
      dataCourses: [],
      course: "",
      courseDetails: [],
      dataProgram: [],
    };
  },
  mounted() {
    this.fetchData("https://rest-courses-api.herokuapp.com/api/v1/courses")
      .then((data) => {
        this.dataCourses = data.data;
      })
      .catch((err) => console.log(err));
  },
  computed: {},
  methods: {
    fetchData(url) {
      var response = fetch(url, {
        method: "GET",
      })
        .then((data) => data.json())
        .then((data) => {
          return data;
        });
      return response;
    },
    fetchCourse(data) {
      this.course = data;
      var url = "https://rest-courses-api.herokuapp.com/api/v1/courses/" + data;
      fetch(url, {
        method: "GET",
      })
        .then((data) => data.json())
        .then((data) => {
          this.courseDetails = data.data;
          this.dataProgram = data.included;
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style scope>
</style>