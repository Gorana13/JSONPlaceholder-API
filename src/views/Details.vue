<template>
  <div class="details">
    <div id="nav">
      <router-link to="/">Home</router-link>
      <router-view />
    </div>
    <div v-if="hasData" id="data">
      <h1>Title: {{ details.title }}</h1>
      <h2>Text: {{ details.body }}</h2>
    </div>
    <div v-else>
      <h1>No data</h1>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      id: this.$route.params.id,
      url: "https://jsonplaceholder.typicode.com/posts/",
      details: {},
      hasData: false,
    };
  },
  mounted() {
    this.getDetails();
  },
  methods: {
    getDetails() {
      this.axios
        .get(this.url + this.id)
        .then((resp) => {
          this.details = resp.data;
          this.hasData = true;
        })
        .catch((err) => {
          if (err.response.status == "404") {
            console.warn(err);
          }
        });
    },
  },
};
</script>
<style >
#data,
h1,
h2 {
  background: #e1f5fe;
  margin: 4rem;
}
#nav > a {
  font-size: 2rem;
  margin: 30px;
  text-decoration: none;
}
</style>

