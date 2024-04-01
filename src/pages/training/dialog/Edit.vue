<template>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title class="text-h5">Edit Train</v-card-title>
        <v-card-text>
          <v-text-field v-model="editedTrain.fullName" label="Full Name">
          </v-text-field>
          <v-text-field v-model="editedTrain.teacher" label="Teacher"></v-text-field>
          <v-text-field v-model="editedTrain.student" label="Students"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="blue darken-1" @click="saveChanges">Save</v-btn>
          <v-btn color="blue darken-1" @click="closeDialog">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </template>
  
  <script>
  export default {
    props: {
      train: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        dialog: false,
        editedTrain: {
          fullName:''
        }
      };
    },
    computed: {
    fullName() {
      return `${this.editedTrain.firstName} ${this.editedTrain.lastName}`;
    }
  },
    methods: {
      openDialog() {
        this.dialog = true;
        this.editedTrain = { ...this.train, fullName: this.fullName };
      },
      saveChanges() {
        const [firstName, lastName] = this.editedTrain.fullName.split(' ');
      this.editedTrain.firstName = firstName;
      this.editedTrain.lastName = lastName;
        this.$emit('save-changes', this.editedTrain);
        this.dialog = false;
      },
      closeDialog() {
        this.dialog = false;
      }
    }
  };
  </script>
  