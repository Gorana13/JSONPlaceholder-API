<template>
  <div>
    <form>
      <v-text-field v-model="username" label="username"></v-text-field>
      <v-text-field v-model="password" label="password"></v-text-field>
    </form>
  </div>
</template>
<script>
export default {
  name: "login",
  data() {
    return {
      username: "",
      password: "",
    };
  },
};
</script>
