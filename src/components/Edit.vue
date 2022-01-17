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
          <form @submit.prevent="onSubmit" method="patch">
            <v-text-field v-model="id" label="ID" readonly> </v-text-field>
            <v-text-field label="Naziv" v-model="title"></v-text-field>
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

Vue.use(VueAxios, axios);

export default {
  name: "Edit",
  props: {
    dialog: {
      default: false,
    },
    editItem: {
      default: null,
      required: true,
    },
  },
  data() {
    return {
      item: {},
      id: null,
      title: null,
      body: null,
    };
  },
  watch: {
    editItem: {
      handler(nv) {
        this.id = nv.id;
        this.title = nv.title;
        this.body = nv.body;
      },
      deep: true,
    },
  },
  methods: {
    close() {
      this.$emit("closeEvent");
    },
    onSubmit() {
      this.item = {
        id: this.id,
        title: this.title,
        body: this.body,
      };
      Vue.axios
        .patch("https://jsonplaceholder.typicode.com/posts/1", this.item)
        .then(() => {
          this.$emit("editDone", this.item);
        })
        .catch((err) => {
          console.log(err);
        });
      this.close();
      // this.fetch();
    },
  },
};
</script>