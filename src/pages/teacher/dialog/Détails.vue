<template>
    <v-dialog v-model="isDialogOpen" max-width="500px">
        <v-card>
          <v-card-title class="text-h5">Teacher Details</v-card-title>
          <v-card-text>
            <p><strong>Name:</strong> {{ teacher.firstName }} {{ teacher.lastName }}</p>
            <p><strong>Email:</strong> {{ teacher.email }}</p>
            <p><strong>Phone:</strong> {{ teacher.phone }}</p>
            <p><strong>Status:</strong> {{ teacher.status }}</p>
          </v-card-text>
          <v-card-actions>
            <v-btn color="blue darken-1" variant="text" @click="closeDialog">Close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </template>
    
    <script>
    export default {
      props: {
        teacher: {
          type: Object,
          required: true
        },
        showDialog: {
          type: Boolean,
          required: true
        }
      },
      computed: {
        isDialogOpen: {
          get() {
            return this.showDialog;
          },
          set(value) {
            if (!value) {
              this.closeDialog();
            }
          }
        },
        },
      methods: {
        closeDialog() {
          this.$emit('update:showDialog', false);
        }
      }
    };
    </script>