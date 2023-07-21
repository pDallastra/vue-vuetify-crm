<template>
  <div class="pa-4">
    <v-data-table
    :headers="headers"
    :items="quotes"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>Cutomers</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="mx-2"
              fab
              dark
              small
              color="primary"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon dark>
                mdi-plus
              </v-icon>
            </v-btn>
          </template>

          <CreateCustomerCard 
            :form-title="formTitle"
            :lastQuoteNumber="quotes[quotes.length -1].quote"
            @save="save"
            @cancel="close"
          />
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
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
        class="mr-2"
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-airplane
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
  </div>

</template>

<script>
import CreateCustomerCard from './FormCardsContent/CreateCustomerCard.vue';

export default {
  name: 'TableDate',

  components: {
    CreateCustomerCard
  },

  data: () => ({
      dialog: false,
      dialogDelete: false,
      headers: [
        {
          text: 'Quote',
          align: 'start',
          value: 'quote',
        },
        { text: 'Customer', value: 'customer' },
        { text: 'Items', value: 'items' },
        { text: 'Price', value: 'price' },
        { text: 'Seller', value: 'seller' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      quotes: [],
      editedIndex: -1,
      defaultItem: {
        quote: '',
        customer: '',
        items: 0,
        price: 0,
        seller: 0,
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Customer' : 'Edit Customer'
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
        this.quotes = [
          {
            quote: '0001',
            customer: 'Apple',
            items: 10,
            price: 3750,
            seller: 'Rogerio Souza'
          },
          {
            quote: '0002',
            customer: 'Samsung',
            items: 7,
            price: 12750.73,
            seller: 'Rogerio Souza'
          },
          {
            quote: '0003',
            customer: 'Microsoft',
            items: 17,
            price: 55482.49,
            seller: 'Rogerio Souza'
          },
          {
            quote: '0004',
            customer: 'Tesla',
            items: 23,
            price: 107482.89,
            seller: 'Fernanda Motta'
          },
          {
            quote: '0005',
            customer: 'Facebook',
            items: 4,
            price: 14482.49,
            seller: 'Fernanda Motta'
          },
          {
            quote: '0006',
            customer: 'Microsoft',
            items: 12,
            price: 97532.49,
            seller: 'Fernanda Motta'
          }
        ]
      },

      editItem (item) {
        this.editedIndex = this.quotes.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        this.editedIndex = this.quotes.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        this.quotes.splice(this.editedIndex, 1)
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

      save (quote) {
        if (this.editedIndex > -1) {
          Object.assign(this.quotes[this.editedIndex], quote)
        } else {
          this.quotes.push(quote)
        }
        this.close()
      },
    },
}
</script>