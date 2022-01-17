<template>
  <v-app>
    <v-dialog
      @click:outside="close()"
      v-model="dialog"
      scrollable
      max-width="80%"
    >
      <v-card>
        <v-card-text>
          <form @submit.prevent="onSubmit" method="post">
            <ValidationProvider
              name="field"
              rules="required"
              v-slot="{ errors }"
            >
              <v-text-field label="Naziv" v-model="title"></v-text-field>
              <span>{{ errors[0] }}</span>
            </ValidationProvider>
            <v-text-field v-model="body" label="Text"></v-text-field>
            <v-btn type="submit"> submit </v-btn>
            <v-btn @click.prevent="close()"> Close </v-btn>
          </form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-app>
</template>
<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
import { ValidationProvider } from "vee-validate";

Vue.use(VueAxios, axios);

export default {
  components: {
    ValidationProvider,
  },
  name: "Add",
  props: {
    dialog: {
      default: false,
    },
  },
  data() {
    return {
      title: null,
      body: null,
      value: null,
      employeeData: {},
    };
  },
  methods: {
    close() {
      this.$emit("closeEvent");
    },
    onSubmit() {
      this.employeeData = {
        title: this.title,
        body: this.body,
      };
      this.title = null;
      this.body = null;
      this.addNew();
    },
    addNew() {
      Vue.axios
        .post(
          "https://jsonplaceholder.typicode.com/posts",
          JSON.stringify(this.employeeData)
        )
        .then(({ data }) => {
          this.employeeData.id = data.id;

          this.$emit("fetchList", this.employeeData);
        })
        .catch((error) => {
          this.errorMessage = error.message;
          console.error("There was an error!", error);
        })
        .finally(() => {
          this.close();
        });
      // this.fetch();
    },
  },
};
</script>
<style scoped>
</style>