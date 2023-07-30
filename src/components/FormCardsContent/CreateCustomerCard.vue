<template>
            <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="12"
                    md="12"
                  >
                  <v-select
                    v-model="quote.customer"
                    :items="customers"
                    density="customer"
                    label="Customer"
                  ></v-select>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="6"
                  >
                  <v-select
                    v-model="quote.seller"
                    :items="sellers"
                    density="seller"
                    label="Seller"
                  ></v-select>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="6"
                  >
                  <v-text-field
                    v-model="quote.items"
                    label="Items"
                    type="number"
                  />
                  </v-col>
                </v-row>

                <v-row
                  v-if="quote.items.length"
                >
                <v-col
                    v-for="index in Number(quote.items)"
                    :key="index"
                    cols="12"
                    sm="12"
                    md="12"
                  >
                  <v-select
                    v-model="quote.item[index]"
                    :items="customers"
                    density="customer"
                    label="Customer"
                  ></v-select>
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
</template>

<script>
export default {
  name: 'CreateCustomerCard',

  data: () => ({
    closingModal: false,
    customers: [
      { text: 'Apple', id: 1}, 
      { text: 'Microsoft', id: 2}, 
      { text: 'Samsung', id: 3},
    ],
    sellers: [
      { text: 'Rogério Souza', id: 1}, 
      { text: 'Fernanda Motta', id: 2}, 
      { text: 'Paulo Dallastra', id: 3},
    ],
    quote: {
      quote: '',
      customer: '',
      items: 0,
      price: 0,
      seller: '',
      item: []
    },
    defaultReset: {
      quote: '',
      customer: '',
      items: 0,
      price: 0,
      seller: '',
    }
  }),

  props: {
    formTitle: {
      type: String,
      default: ''
    },

    lastQuoteNumber: {
      type: [Number, String],
      default: 0
    }
  },

  watch: {
    'quote.items': {
      handler(newValue, oldValue) {
        if (oldValue > newValue && this.closingModal) {
          this.quote.item[this.quote.item.length - 1] = null;
        }

        console.log(this.quote)
      }
    }
  },

  methods: {
    save() {
      const lastQuoteAsNumber = Number(this.lastQuoteNumber);
      this.quote.quote = String(lastQuoteAsNumber + 1).padStart(4, '0');
      this.$emit('save', this.quote);
      this.closingModal = true;
      this.quote  = this.defaultReset;
    },

    close() {
      this.closingModal = true;
      this.quote = this.defaultReset;
      this.$emit('cancel');
    }
  }
}
</script>