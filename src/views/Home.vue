<template>
  <div class="home">
    <div v-if="login">
      <home></home>
    </div>
    <div v-else>
      <login></login>
    </div>
  </div>
</template>

<script>
import Home from "../components/Home";
import Login from "../components/Login";
export default {
  name: "home-view",
  components: {
    Home,
    Login,
  },
  created() {
    this.$cookies.set("login", false);
  },
  data() {
    return {};
  },
  computed: {
    login() {
      return JSON.parse(this.$cookies.get("login"));
    },
  },
};
</script>
