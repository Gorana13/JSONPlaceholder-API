<template>
  <v-app>
    <v-card>
      <v-card-title
        style="font-size: 2em"
        class="justify-center light-blue--text"
        >JSONPlaceholder</v-card-title
      >

      <v-data-table
        id="tableId"
        :headers="headers"
        :items="list"
        :items-per-page="5"
        :search="search"
        class="table elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat color="white">
            <div class="d-flex">
              <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                label="Search"
                dense
                outlined
                single-line
                hide-details
              ></v-text-field>
              <v-btn class="ml-2" @click.prevent="dialogAddShow = true">
                <v-icon dark>mdi-plus</v-icon>Add
              </v-btn>
            </div>
          </v-toolbar>
        </template>

        <template v-slot:item="{ item }">
          <tr class="rows">
            <td>{{ item.id }}</td>
            <td>{{ item.title }}</td>
            <td>{{ item.body }}</td>
            <td class="btns">
              <v-btn
                color="light-blue lighten-5"
                fab
                small
                @click="openDeleteDialog(item.id)"
              >
                <v-icon small> mdi-delete </v-icon>
              </v-btn>

              <v-btn
                color="light-blue lighten-5"
                fab
                small
                @click.prevent="editFn(item)"
              >
                <v-icon small> mdi-pencil </v-icon>
              </v-btn>
            </td>
          </tr>
        </template>
      </v-data-table>
      <!-- Error dialog when adding -->
      <v-dialog v-model="errorDialog" max-width="500px">
        <v-card>
          <v-card-title>
            <span>Error, id already exists!</span>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="errorDialog = false">
              Close
            </v-btn>
          </v-card-title>
        </v-card></v-dialog
      >
      <!-- Warning dialog when deleting -->
      <v-dialog v-model="deleteDialog" max-width="500px">
        <v-card>
          <v-card-title>
            <span>Are you sure you want to delete this item?</span>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="deleteItem(deleteId)">
              Delete
            </v-btn>
            <v-btn color="primary" text @click="deleteDialog = false">
              Close
            </v-btn>
          </v-card-title>
        </v-card></v-dialog
      >
    </v-card>
    <add
      @fetchList="addToList"
      :addDialog="dialogAddShow"
      @closeEvent="closeFcn"
    ></add>
    <edit
      :editItem="editTemplate"
      :editDialog="dialogEditShow"
      @closeEvent="closeFcn"
      @editDone="editTable"
    ></edit>
  </v-app>
</template>


<script>
import Add from "./Add";
import Edit from "./Edit";
export default {
  name: "Home",
  components: {
    Add,
    Edit,
  },
  data() {
    return {
      list: [],
      errorDialog: false,
      deleteDialog: false,
      editTemplate: {
        id: null,
        title: null,
        body: null,
      },
      editIndex: null,
      deleteId: null,
      search: "",
      dialogAddShow: false,
      dialogEditShow: false,
      headers: [
        {
          text: "ID",
          value: "id",
          width: "5rem",
          class: "light-blue lighten-2  white--text",
        },
        {
          text: "Naziv",
          value: "title",
          class: "light-blue lighten-2 white--text",
        },
        {
          text: "Text",
          value: "body",
          class: "light-blue lighten-2  white--text",
        },
        {
          text: "",
          value: "action",
          width: "8rem",
          class: "light-blue lighten-2  white--text",
        },
      ],
    };
  },
  mounted() {
    this.fetch();
  },
  methods: {
    closeFcn() {
      this.dialogAddShow = false;
      this.dialogEditShow = false;
    },
    editFn(item) {
      this.dialogEditShow = true;
      this.editTemplate.id = item.id;
      this.editTemplate.body = item.body;
      this.editTemplate.title = item.title;
    },
    fetch() {
      this.axios
        .get("https://jsonplaceholder.typicode.com/posts")
        .then((resp) => {
          this.list = resp.data;
        })
        .catch((err) => {
          console.warn(err);
        });
    },
    errorFnc() {
      this.errorDialog = true;
    },
    addToList(nv) {
      if (!this.list.some((element) => element.id === nv.id)) {
        this.list.push(nv);
      } else {
        this.errorFnc();
      }
    },
    deleteItem(id) {
      this.axios
        .delete(`https://jsonplaceholder.typicode.com/posts/${id}`)
        .then(() => {
          this.list = this.list.filter((list) => list.id !== id);
          // this.fetch();
        })
        .finally(() => {
          this.deleteDialog = false;
        });
    },
    editTable(item) {
      this.editIndex = this.list.findIndex((list) => list.id === item.id);
      this.list[this.editIndex].body = item.body;
      this.list[this.editIndex].title = item.title;
    },
    openDeleteDialog(id) {
      this.deleteDialog = true;
      this.deleteId = id;
    },
  },
};
</script>

<style scoped>
.table {
  margin: 50px;
}
* >>> .btns {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}
* >>> .rows {
  background-color: #fafafa;
}
</style>
