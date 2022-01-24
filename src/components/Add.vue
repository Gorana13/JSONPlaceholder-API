<template>
  <v-app>
    <v-dialog
      @click:outside="close()"
      v-model="addDialog"
      scrollable
      max-width="80%"
    >
      <v-card>
        <v-card-text>
          <v-form @submit.prevent="onSubmit" ref="frm" method="post">
            <v-text-field
              label="Naziv"
              :rules="[rules.required, rules.counter]"
              v-model="title"
            ></v-text-field>
            <v-text-field
              v-model="body"
              label="Text"
              :rules="[rules.required]"
            ></v-text-field>
            <v-btn type="submit"> submit </v-btn>
            <v-btn @click.prevent="close()"> Close </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-app>
</template>
<script>
export default {
  name: "Add",
  props: {
    addDialog: {
      default: false,
    },
  },
  data() {
    return {
      title: null,
      body: null,
      value: null,
      employeeData: {},
      rules: {
        required: (value) => !!value || "Required.", //!!null === false
        counter: (value) => value?.length <= 20 || "Max 20 characters",
      },
    };
  },
  methods: {
    close() {
      this.$emit("closeEvent");
    },
    onSubmit() {
      if (!this.$refs.frm.validate()) {
        return;
      }
      this.$refs.frm.resetValidation();
      this.employeeData = {
        title: this.title,
        body: this.body,
      };
      this.title = null;
      this.body = null;
      this.addNew();
    },
    addNew() {
      this.axios
        .post(
          "https://jsonplaceholder.typicode.com/posts",
          JSON.stringify(this.employeeData)
        )
        .then(({ data }) => {
          this.employeeData.id = data.id;

          this.$emit("fetchList", this.employeeData);
        })
        .catch((error) => {
          console.error(error.message);
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