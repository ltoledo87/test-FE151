<template>
  <b-container>
    <b-row class="py-3">
      <b-col>
        <b-avatar
          rounded
          v-bind:src="userProfile.avatar"
          size="6rem"
        ></b-avatar>
      </b-col>
    </b-row>
    <b-row class="py-3">
      <b-col>
        <b-card header="Mis datos" header-tag="header">
          <b-card-text
            ><strong
              >{{ userProfile.firstname }} {{ userProfile.lastname }}</strong
            ></b-card-text
          >
          <b-card-text>{{ dataUser.email }} </b-card-text>
          <b-card-text>
            <b-row>
              <b-col>
                <b-icon icon="github" font-scale="2" aria-hidden="true"></b-icon
                ><a v-bind:href="github.url">{{ github.name }}</a>
              </b-col>
              <b-col>
                <b-icon
                  icon="twitter"
                  font-scale="2"
                  aria-hidden="true"
                ></b-icon
                ><a v-bind:href="twiter.url">{{ twiter.name }}</a>
              </b-col>
            </b-row>
          </b-card-text>
        </b-card>
      </b-col>
    </b-row>
    <b-row class="py-3">
      <b-col>
        <b-card header="Mis Cursos" header-tag="header">
          <b-card-text>
            <b-row
              v-for="course in courseProfile"
              v-bind:key="course.attributes.slug"
              class="py-1"
            >
              <b-col class="text-left" cols="8">
                {{ course.attributes.name }}
              </b-col>
              <b-col cols="4">
                <b-button
                  v-b-modal="'myModal-' + course.attributes.slug"
                  variant="primary"
                  >Detalles</b-button
                >
              </b-col>
              <b-modal
                :id="'myModal-' + course.attributes.slug"
                size="lg"
                title="Información del Curso"
                ok-only
              >
                <b-container class="text-center">
                  <b-row class="py-3">
                    <b-col>
                      <b-avatar
                        square
                        :src="course.attributes.cover"
                        size="6rem"
                        variant="light"
                      ></b-avatar
                      ><br />
                      {{ course.attributes.name }}
                    </b-col>
                  </b-row>
                  <b-row class="py-3">
                    <b-col>
                      <b-card header="Aprendizaje" header-tag="header">
                        <b-card-text
                          v-for="courseDetails in course.attributes.learning"
                          v-bind:key="courseDetails.name"
                        >
                          <b-row>
                            <b-col cols="6" class="text-left">
                              <strong>{{ courseDetails.name }}</strong
                              ><br />
                              {{ courseDetails.status }}
                            </b-col>
                            <b-col cols="6">
                              <h6>Videos</h6>
                              <b-progress
                                :max="courseDetails.videos.total"
                                height="1rem"
                                striped
                                :animated="true"
                              >
                                <b-progress-bar
                                  :value="courseDetails.videos.progress"
                                >
                                  <span
                                    >Progreso:
                                    <strong
                                      >{{ courseDetails.videos.progress }} /
                                      {{ courseDetails.videos.total }}</strong
                                    ></span
                                  >
                                </b-progress-bar>
                              </b-progress>
                              <h6>Evaluaciones</h6>
                              <b-progress
                                :max="courseDetails.assessments.total"
                                height="1rem"
                                variant="success"
                                striped
                                :animated="true"
                              >
                                <b-progress-bar
                                  :value="courseDetails.assessments.progress"
                                >
                                  <span
                                    >Progreso:
                                    <strong
                                      >{{ courseDetails.assessments.progress }}
                                      /
                                      {{
                                        courseDetails.assessments.total
                                      }}</strong
                                    ></span
                                  >
                                </b-progress-bar>
                              </b-progress>
                            </b-col>
                          </b-row>
                          <hr />
                        </b-card-text>
                      </b-card>
                    </b-col>
                  </b-row>
                </b-container>
              </b-modal>
            </b-row>
          </b-card-text>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "Profile",
  data() {
    return {
      dataUser: [],
      dataProfile: [],
      userProfile: [],
      github: [],
      twiter: [],
      courseProfile: [],
    };
  },
  mounted() {
    this.fetchData("https://rest-courses-api.herokuapp.com/api/v1/users/me")
      .then((data) => {
        this.dataUser = data.data["attributes"];
        this.dataProfile = data.included;
        this.filterProfile();
        this.filterSocial();
        this.filterCourse();
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
    filterProfile() {
      var profile = this.dataProfile.filter((item) => item.type == "profile");
      profile = profile[0]["attributes"];
      this.userProfile = profile;
    },
    filterSocial() {
      var social = this.dataProfile.filter(
        (item) => item.type == "social_network"
      );
      var github = social[0]["attributes"];
      var twiter = social[1]["attributes"];
      this.github = github;
      this.twiter = twiter;
    },
    filterCourse() {
      this.courseProfile = this.dataProfile.filter(
        (item) => item.type == "course"
      );
    },
  },
};
</script>

<style scope>
</style>