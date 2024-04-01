<template>
  <v-data-table :headers="headers" :items="teachers" :sort-by="[{ key: 'email', order: 'asc' }]">
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>My CRUD</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <Comp @saveTeacher="handleNewTeacher" />
      </v-toolbar>
    </template>

    <template te v-slot:item.name="{ item }">
      <span>{{ item.firstName }} {{ item.lastName }}</span>
    </template>

    <template v-slot:item.actions="{ item }">
      <v-icon class="me-2" size="small" @click="editItem(item)">mdi-pencil</v-icon>
      <v-icon size="small" @click="deleteItem(item)">mdi-delete</v-icon>
      <v-icon size="small" @click="detailsItem(item)">mdi-details</v-icon>
      
    </template>

    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>

  <v-dialog v-model="dialogDelete" max-width="500px">
    <v-card>
      <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
        <v-btn color="blue darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
        <v-spacer></v-spacer>
      </v-card-actions>
    </v-card>
  </v-dialog>
  <Détails :teacher="editedItem" :showDialog="dialogDetails" @update:showDialog="updateDialogDetails" />
  <Edit :teacher="editedItem" @save-changes="handleUpdateStudent" ref="editItemDialog" />

</template>

<script>
import Comp from "./Add.vue";
import Détails from "./Détails.vue";
import Edit from "./Edit.vue"
export default {
  components: {
    Comp,
    Détails,
    Edit
  },
  data() {
    return {
      teachers: [],
      dialog: false,
      dialogDelete: false,
      editedItem: {},
      dialogDetails: false,
      headers: [
        {
          title: "Teacher",
          align: "start",
          sortable: false,
          value: "name"
        },
        { title: "Email", value: "email" },
        { title: "Phone", value: "phone" },
        { title: "Status", value: "status" },
        { title: "Actions", value: "actions", sortable: false }
      ]
    };
  },
  created() {
    this.initialize();
  },
  methods: {
    initialize() {
      this.teachers = [
        {
          firstName: "Kamel",
          lastName: "Mrabet",
          email: "kamelmrabet@gmail.com",
          phone: "0545120854",
          status: 'single'
        },
        {
          firstName: "Cristiano",
          lastName: "Ronaldo",
          email: "cristianoronaldo@gmail.com",
          phone: "0698547122",
          status: 'Married'
        },
        {
            firstName: "Mike",
            lastName: "Tyson",
          email: "miketyson@gmail.com",
          phone: "0632589614",
          status: 'Dévorced'
        }
      ];
    },
    editItem(item) {
      this.editedIndex = this.teachers.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.$nextTick(() => {
        this.$refs.editItemDialog.openDialog(); 
      }); 
    },
    deleteItem(item) {
      this.editedIndex = this.teachers.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },
    closeDelete() {
      this.dialogDelete = false;
    },
    deleteItemConfirm() {
      this.teachers.splice(this.editedIndex, 1);
      this.closeDelete();
    },
    detailsItem(item) {
      this.editedItem = Object.assign({}, item); 
      this.dialogDetails = true; 
    },
    closeDetails() {
      this.dialogDetails = false;
    },
    updateDialogDetails(value) {
      this.dialogDetails = value;
    },
    handleUpdateStudent(updatedStudent) {
  if (this.editedIndex !== undefined && this.editedIndex !== null) {
    this.teachers.splice(this.editedIndex, 1, updatedStudent);
  }
},
    handleNewTeacher(TeacherData) {
      if (TeacherData) {
        this.teachers.push(TeacherData);
      } else {
        console.error("Error: Teacher data not received from NewItemCom.vue");
      }
    }
  }
};
</script>
