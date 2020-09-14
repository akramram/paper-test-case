<template>
  <v-container fluid>
    <v-card outlined>
      <v-tabs>
        <v-tab>
          account
        </v-tab>
        <v-tab>
          transaction
        </v-tab>

        <v-tab-item>
          <v-card-text>
            <v-data-table
              :headers="headersacc"
              :items="accounts"
              :custom-filter="filterOnlyCapsText"
            >
              <template v-slot:top>
                <v-toolbar flat color="white">
                  <v-text-field v-model="search2" label="Search"></v-text-field>

                  <v-spacer></v-spacer>
                  <v-dialog v-model="dialog2" max-width="500px">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn
                        color="primary"
                        dark
                        class="mb-2"
                        v-bind="attrs"
                        v-on="on"
                        >New Item</v-btn
                      >
                    </template>
                    <v-card>
                      <form @submit.prevent="save2">
                        <v-card-title>
                          <span class="headline">{{ formTitle2 }}</span>
                        </v-card-title>
                        <v-card-text>
                          <div>
                            <label>name</label>
                            <v-text-field
                              v-model="editedItem2.name"
                              :readonly="readOnly"
                              outlined
                              type="text"
                            />
                          </div>
                          <div>
                            <label>description</label>
                            <v-text-field
                              v-model="editedItem2.Description"
                              :readonly="readOnly"
                              outlined
                              type="text"
                            />
                          </div>
                          <div>
                            <label>type</label>
                            <v-text-field
                              v-model="editedItem2.type"
                              :readonly="readOnly"
                              outlined
                              type="text"
                            />
                          </div>
                        </v-card-text>
                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn color="blue darken-1" text @click="close2">{{
                            readOnly ? 'Close' : 'Cancel'
                          }}</v-btn>
                          <v-btn
                            v-if="!readOnly"
                            color="blue darken-1"
                            text
                            type="submit"
                            >Save</v-btn
                          >
                        </v-card-actions>
                      </form>
                    </v-card>
                  </v-dialog>
                </v-toolbar>
              </template>
              <template v-slot:item.actions="{ item }">
                <v-icon small class="mr-2" @click="viewItem2(item)">
                  remove_red_eye
                </v-icon>
                <v-icon small class="mr-2" @click="editItem2(item)">
                  edit
                </v-icon>
                <v-icon small @click="deleteItem2(item)">
                  delete
                </v-icon>
              </template>
              <!-- <template v-slot:no-data>
                <v-btn color="primary" @click="initialize">Reset</v-btn>
              </template> -->
            </v-data-table>
          </v-card-text>
        </v-tab-item>

        <v-tab-item>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="finances"
              :custom-filter="filterOnlyCapsText"
            >
              <template v-slot:top>
                <v-toolbar flat color="white">
                  <v-text-field v-model="search" label="Search"></v-text-field>

                  <v-spacer></v-spacer>
                  <v-dialog v-model="dialog" max-width="500px">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn
                        color="primary"
                        dark
                        class="mb-2"
                        v-bind="attrs"
                        v-on="on"
                        >New Item</v-btn
                      >
                    </template>
                    <v-card>
                      <form @submit.prevent="addFinance">
                        <v-card-title>
                          <span class="headline">{{ formTitle }}</span>
                        </v-card-title>

                        <v-card-text>
                          <v-container>
                            <div>
                              <label>Title</label>
                              <v-text-field
                                v-model="editedItem.title"
                                outlined
                                type="text"
                              />
                            </div>
                            <div>
                              <label>debit_ammount</label>
                              <v-text-field
                                v-model="editedItem.debit_ammount"
                                outlined
                                type="text"
                              />
                            </div>
                            <div>
                              <label>credit_ammount</label>
                              <v-text-field
                                v-model="editedItem.credit_ammount"
                                outlined
                                type="text"
                              />
                            </div>
                            <div>
                              <label for="">account id</label>
                              <v-autocomplete
                                v-model="select"
                                outlined
                                :loading="loading"
                                :items="accounts"
                                item-text="name"
                                item-value="id"
                                hide-no-data
                                hide-details
                              />
                            </div>
                            <div>
                              <label>desc</label>
                              <v-text-field
                                v-model="editedItem.description"
                                outlined
                                type="text"
                              />
                            </div>
                            <!-- <div>
                              <v-btn color="primary" type="submit"
                                >Submit</v-btn
                              >
                            </div> -->
                          </v-container>
                        </v-card-text>

                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn color="blue darken-1" text @click="close">{{
                            readOnly ? 'Close' : 'Cancel'
                          }}</v-btn>
                          <v-btn
                            v-if="!readOnly"
                            color="blue darken-1"
                            text
                            type="submit"
                            >Save</v-btn
                          >
                        </v-card-actions>
                      </form>
                    </v-card>
                  </v-dialog>
                </v-toolbar>
              </template>
              <template v-slot:item.actions="{ item }">
                <v-icon small class="mr-2" @click="viewItem(item)">
                  remove_red_eye
                </v-icon>
                <v-icon small class="mr-2" @click="editItem(item)">
                  edit
                </v-icon>
                <v-icon small @click="deleteItem(item)">
                  delete
                </v-icon>
              </template>
              <!-- <template v-slot:no-data>
                <v-btn color="primary" @click="initialize">Reset</v-btn>
              </template> -->
            </v-data-table>
          </v-card-text>
        </v-tab-item>
      </v-tabs>
    </v-card>
  </v-container>
</template>
<script>
export default {
  data: () => ({
    dialog: false,
    dialog2: false,
    finances: [],
    search: '',
    search2: '',
    select: '',
    readOnly: false,
    headers: [
      {
        text: 'title',
        align: 'start',
        sortable: false,
        value: 'title'
      },
      { text: '​debit_amount​', value: '​debit_ammount​' },
      { text: '​credit_amount​', value: 'credit_ammount' },
      { text: 'description', value: 'description' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    accounts: [],
    headersacc: [
      {
        text: 'name',
        align: 'start',
        sortable: false,
        value: 'name'
      },
      { text: 'description', value: 'Description' },
      { text: 'type', value: 'type' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    desserts: [],
    editedIndex: -1,
    editedIndex2: -1,
    editedItem: {},
    editedItem2: {},
    defaultItem: {},
    defaultItem2: {}
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Add Finance' : 'Edit Finance'
    },
    formTitle2() {
      return this.editedIndex2 === -1 ? 'Add Account' : 'Edit Account'
    }
  },

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialog2(val) {
      val || this.close2()
    }
  },

  created() {
    this.getFinance()
    this.getaccountlist()
  },

  methods: {
    filterOnlyCapsText(value, search, item) {
      return (
        value != null &&
        search != null &&
        typeof value === 'string' &&
        value.toString().includes(search)
      )
    },
    viewItem(item) {
      this.editedIndex = this.finances.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
      this.readOnly = true
    },
    editItem(item) {
      this.editedIndex = this.finances.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      confirm('Are you sure you want to delete this item?') &&
        this.deleteFinance(item.id)
    },

    close() {
      this.dialog = false
      this.readOnly = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save() {
      if (this.editedIndex > -1) {
        this.editFinance(this.editedIndex)
      } else {
        this.addFinance()
      }
      this.close()
    },

    async getFinance() {
      const datas = await this.$axios.get('finances', {
        params: {
          sort_field: 'name',
          sort_type: 1,
          page: 1,
          per_page: 20
        }
      })
      this.finances = datas.data.data
    },
    async addFinance() {
      this.finances.push(this.editedItem)
      try {
        await this.$axios.post('finances', {
          title: this.editedItem.title,
          debit_ammount: this.editedItem.debit_ammount,
          credit_ammount: this.editedItem.credit_ammount,
          description: this.editedItem.description,
          finance_account_id: this.select
        })
        // this.getFinance()
        this.close()
      } catch (err) {
        // eslint-disable-next-line no-console
        console.log(err)
      }
    },
    async editFinance(id) {
      Object.assign(this.finances[this.editedIndex], this.editedItem)
      try {
        await this.$axios.patch('finances/' + this.editedItem.id, {
          title: this.editedItem.title,
          debit_ammount: this.editedItem.debit_ammount,
          credit_ammount: this.editedItem.credit_ammount,
          description: this.editedItem.description,
          finance_account_id: 56
        })
        // this.getFinance()
        this.close()
      } catch (err) {
        // eslint-disable-next-line no-console
        console.log(err)
      }
    },
    async deleteFinance(id) {
      try {
        await this.$axios.delete('finance/' + id)
        // this.getFinance()
        this.getFinance()
      } catch (err) {
        // eslint-disable-next-line no-console
        console.log(err)
      }
    },

    viewItem2(item) {
      this.editedIndex2 = this.accounts.indexOf(item)
      this.editedItem2 = Object.assign({}, item)
      this.dialog2 = true
      this.readOnly = true
    },
    editItem2(item) {
      this.editedItem2 = Object.assign({}, item)
      this.editedIndex2 = this.editedItem2.id
      this.dialog2 = true
    },

    deleteItem2(item) {
      //   const index = this.accounts.indexOf(item)
      confirm('Are you sure you want to delete this item?') &&
        this.deleteAccount(item.id)
    },

    close2() {
      this.dialog2 = false
      this.readOnly = false
      this.$nextTick(() => {
        this.editedItem2 = Object.assign({}, this.defaultItem)
        this.editedIndex2 = -1
      })
    },

    save2() {
      if (this.editedIndex2 > -1) {
        this.editAccount(this.editedIndex2)
      } else {
        this.addAccount()
      }
    },

    async addAccount() {
      this.accounts.push(this.editedItem2)
      try {
        await this.$axios.post('finance-accounts', {
          name: this.editedItem2.name,
          type: this.editedItem2.type,
          Description: this.editedItem2.Description
        })
        this.close2()
        // this.getaccountlist()
      } catch (error) {
        // eslint-disable-next-line no-console
        console.log(error)
      }
    },
    async getaccountlist() {
      try {
        const datas = await this.$axios.get('finance-accounts', {
          params: {
            sort_field: 'name',
            sort_type: '1',
            page: 1,
            per_page: 20
          }
        })
        this.accounts = datas.data.data
      } catch (error) {
        // eslint-disable-next-line no-console
        console.log(error)
      }
    },
    async editAccount(id) {
      //   Object.assign(this.accounts[this.editedIndex2], this.editedItem2)
      try {
        await this.$axios.put('finance-accounts/' + id, {
          name: this.editedItem2.name,
          type: this.editedItem2.type,
          description: this.editedItem2.Description
        })
        // this.getFinance()
        this.close2()
      } catch (err) {
        // eslint-disable-next-line no-console
        console.log(err)
      }
    },
    async deleteAccount(id) {
      try {
        await this.$axios.delete('finance-accounts/' + id)
        // this.getFinance()
        this.getaccountlist()
      } catch (err) {
        // eslint-disable-next-line no-console
        console.log(err)
      }
    }
  }
}
</script>

<style></style>
