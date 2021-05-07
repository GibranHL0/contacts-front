<template>
  <div data-app class=" px-10 py-10">
      <v-data-table
    :headers="headers"
    :items="contacts"
    :search="search"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Contacts</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
        ></v-text-field>
        <v-spacer></v-spacer>
        <v-btn
              color="primary"
              dark
              @click="toggleModal = !toggleModal"
            >
              Add A New Contact
        </v-btn>
        <v-dialog v-model="dialog" max-width="500px">
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.email"
                      label="Email"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.name"
                      label="Name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.last_name"
                      label="Last Name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.company"
                      label="Company"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.phone"
                      label="Phone"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
  <div v-if="toggleModal" class="fixed overflow-x-hidden overflow-y-auto inset-0 flex justify-center items-center z-50">
        <div class="relative mx-auto w-auto max-w-2xl">
          <div key="Form" class="pl-20 pt-20">
            <div class="flex items-center h-screen w-full">
                <div class="w-full bg-white rounded shadow-lg p-8 m-4 md:max-w-sm md:mx-auto">
                    <h1 class="block w-full text-center text-black mb-6 text-2xl font-medium">
                        New Contact
                    </h1>
                    <form id="form" class="mb-4 md:flex md:flex-wrap md:justify-between">
                        <div class="flex flex-col mb-4 md:w-1/2">
                            <label class="font-medium text-lg text-gray-900 mb-2" for="first_name">
                                Name *
                            </label>
                            <label v-if="name_error" class="font-light text-red-600">Invalid Name</label>
                            <input class="py-2 px-3 text-gray-900 md:mr-2" type="text" v-model="name" maxlength="40" pattern="[a-zA-Z]*" required>
                        </div>
                        <div class="flex flex-col mb-4 md:w-1/2">
                            <label class="font-medium text-lg text-gray-900 mb-2 md:ml-2" for="last_name">
                                Last Name *
                            </label>
                            <label v-if="last_name_error" class="font-light text-red-600">Invalid Last Name</label>
                            <input class="border py-2 px-3 text-gray-900 md:ml-2" type="text" v-model="last_name" maxlength="40" pattern="[a-zA-Z]*" required>
                        </div>
                        <div class="flex flex-col mb-4 md:w-full">
                            <label class="font-medium text-lg text-gray-900 mb-2" for="company">
                                Company
                            </label>
                            <input class="border py-2 px-3 text-gray-900" type="text" v-model="company" maxlength="40">
                        </div>
                        <div class="flex flex-col mb-4 md:w-full">
                            <label class="font-medium text-lg text-gray-900 mb-2" for="phone">
                                Phone
                            </label>
                            <input class="border py-2 px-3 text-gray-900" type="number" v-model="phone" max="1000000000000000000000">
                        </div>
                        <div class="flex flex-col mb-4 md:w-full">
                            <label class="font-medium text-lg text-gray-900 mb-2" for="email">
                                Email *
                            </label>
                            <label v-if="email_error" class="font-light text-red-600">Email Already Exists</label>
                            <label v-if="email_format" class="font-light text-red-600">Invalid Email</label>
                            <input class="border py-2 px-3 text-gray-900" type="email" maxlength="40" v-model="email" required>
                            <p class="text-sm py-4">* Required</p>
                        </div>
                        <button class="bg-black hover:bg-red-400 mx-auto text-white px-2 py-2 rounded-md" type="button" @click="sendInfo()">
                          Create Account
                        </button>
                    </form>
                      <button class="pt-4 block no-underline text-sm w-full text-gray-900 text-center hover:underline" @click="toggleModal=false; resetValidation(); resetValues()">
                        Cancel
                      </button>
                </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="toggleModal" class="absolute z-40 inset-0 opacity-25 bg-black"></div>
  </div>
</template>

<script>

import axios from 'axios'

  export default {
    data: () => ({
      search: '',
      toggleModal: false,
      dialog: false,
      dialogDelete: false,
      email_error: false,
      email_format: false,
      name_error: false,
      last_name_error: false,
      name : '',
      last_name: '',
      company: '',
      phone: '',
      email: '',
      headers: [
        {
          text: 'Email',
          align: 'start',
          sortable: true,
          value: 'email',
        },
        { text: 'Name', value: 'name' },
        { text: 'Last name', value: 'last_name' },
        { text: 'Company', value: 'company' },
        { text: 'Phone', value: 'phone' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      contacts: [],
      editedIndex: -1,
      editedItem: {
        email: '',
        name: '',
        last_name: '',
        company: '',
        phone: '',
      },
      defaultItem: {
        email: '',
        name: '',
        last_name: '',
        company: '',
        phone: '',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        axios.get("https://contacts-api-gibran-academy.herokuapp.com/contacts")
      .then(
        (response) => {
            this.contacts = response.data
        }
      )
      },

      editItem (item) {
        this.editedIndex = this.contacts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        this.editedIndex = this.contacts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        var deleted_contact = this.contacts[this.editedIndex]
        this.contacts.splice(this.editedIndex, 1)
        this.deleteInfo(deleted_contact)
        this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.contacts[this.editedIndex], this.editedItem)
        } else {
          this.contacts.push(this.editedItem)
        }
        var edited_contact = this.contacts[this.editedIndex]
        this.modifyInfo(edited_contact)
        this.close()
      },

      check(){
      var form = document.getElementById('form');
      if(form.checkValidity() == true){
        this.sendInfo()
      }
     },

      sendInfo(){
        this.resetValidation()
        axios.post("https://contacts-api-gibran-academy.herokuapp.com/contacts", {
          email: this.email,
          name: this.name,
          last_name: this.last_name,
          company: this.company, 
          phone: this.phone
        })
        .then(response => {
          if (response.data.msg == "Email already exists"){
            this.email_error = true;
          }
          else if(response.data.msg == "Email is not valid"){
              this.email_format = true
          }
          else if(response.data.msg == "Name is not valid"){
              this.name_error = true
          }
          else if(response.data.msg == "Last Name is not valid"){
              this.last_name_error = true
          }
          else{
            var new_contact = {
                email: response.data.email,
                name: response.data.name,
                last_name: response.data.last_name,
                company: response.data.company,
                phone: response.data.phone
            }
            this.contacts.push(new_contact)
            this.toggleModal = false;
            this.email_error = false;
            this.resetValues()
          }
        });
      },

      resetValues(){
        this.email = '';
        this.name = '';
        this.last_name = '';
        this.company = '';
        this.phone = '';
      },

      resetValidation(){
        this.email_error = false;
        this.email_format = false;
        this.name_error = false;
        this.last_name_error = false;
      },

      modifyInfo(contact){
          axios.put("https://contacts-api-gibran-academy.herokuapp.com/contacts", {
            email: contact.email,
            name: contact.name,
            last_name: contact.last_name,
            company: contact.company, 
            phone: contact.phone
          })
      },

      deleteInfo(contact){
          var uri = "https://contacts-api-gibran-academy.herokuapp.com/contacts/" + contact.email
          axios.delete(uri);
      }
    },
  }
</script>