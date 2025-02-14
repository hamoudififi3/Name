<template>
  <v-dialog v-model="dialog" max-width="500px">
    <template v-slot:activator="{ props }">
      <v-btn class="mb-2" color="primary" dark v-bind="props">New Item</v-btn>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">{{ formTitle }}</span>
      </v-card-title>

      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12" md="4" sm="6">
              <v-text-field
                v-model="editedItem.name.firstName"
                label="First Name"
                :rules="[name => !!name || 'First Name is required']"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field
                v-model="editedItem.name.lastName"
                label="Last Name"
                :rules="[name => !!name || 'Last Name is required']"
              >
                <template v-slot:error="{ input }">
                  <div class="error--text" v-if="!input.length">Name cannot be empty.</div>
                </template>
              </v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field
                v-model="editedItem.email"
                label="Email"
                :rules="[email => !!email || 'Email is required', email => /^\S+@\S+\.\S+$/.test(email) || 'Invalid email format']"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field v-model="editedItem.phone" :rules="[validatePhone]" label="Phone"></v-text-field>
            </v-col>
            <v-col cols="12" md="4" sm="6">
              <v-text-field v-model="editedItem.status" :rules="[PersonStatus]" label="Status"></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" variant="text" @click="close">Cancel</v-btn>
        <v-btn color="blue darken-1" variant="text" :disabled="!validateForm()" @click="save">Save</v-btn>
        <v-spacer></v-spacer>
      </v-card-actions>
    </v-card>
  </v-dialog>
  
</template>

<script>

export default {
  data() {
    return {
      dialog: false,
      formTitle: true,
      editedIndex: -1,
      teachers: [],
      editedItem: {
        name: {
          firstName: "",
          lastName: ""
        },
        email: "",
        phone: 0,
        status: ""
      },
      defaultItem: {
        name: {
          firstName: "",
          lastName: ""
        },
        email: "",
        phone: 0,
        status: ""
      }
    };
  },
  methods: {
    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
        this.formTitle = true;
      });
    },
    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.teachers[this.editedIndex], this.editedItem);
      } else if (this.validateForm() && this.teachers) {
        this.$emit("saveTeacher", this.editedItem);
      } else {
        console.error("Error: Teacher array is not yet initialized");
      }
      this.close();
    },
    validateForm() {
      return (
        this.editedItem.name.firstName &&
        this.editedItem.name.lastName &&
        this.validateEmail(this.editedItem.email) &&
        this.validatePhone(this.editedItem.phone)
      );
    },
    validateEmail(email) {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return emailRegex.test(email);
    },
    PersonStatus(value) {
      const validStatuses = ['Married', 'Single', 'Divorced'];
      if (!validStatuses.includes(this.status)) {
        return 'Status must be Married, Single, or Divorced';
      }
      return true; // Validation passed successfully
    },
    validatePhone(phone) {  
      return (
        (phone.length > 9 && /^(06|05|07)\d+$/.test(phone)  ) ||
        "Invalid phone number: Must be non-empty and contain only digits And More Then 9 digits"
      );
    },
    detailsItem(item) {
      this.$emit('showDialog', true); // Emit event to show student details dialog
    }
  }
};
</script>
