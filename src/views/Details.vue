<template>
  <div class="details">
    <div id="nav">
      <router-link to="/">Home</router-link>
      <router-view />
    </div>
    <div id="data">
      <h1>Title: {{ detailList.title }}</h1>
      <h2>Text: {{ detailList.body }}</h2>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      id: this.$route.params.id,
      url: "https://jsonplaceholder.typicode.com/posts/",
      detailList: [],
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
          this.detailList = resp.data;
        })
        .catch((err) => {
          console.warn(err);
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

