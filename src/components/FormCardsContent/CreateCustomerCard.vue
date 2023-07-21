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
                    sm="6"
                    md="6"
                  >
                    <v-text-field
                      v-model="quote.customer"
                      label="Customer"
                    ></v-text-field>
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
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="quote.price"
                      label="Price"
                      type="number"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="quote.seller"
                      label="Seller"
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
</template>

<script>
export default {
  name: 'CreateCustomerCard',

  data: () => ({
    quote: {
      quote: '',
      customer: '',
      items: 0,
      price: 0,
      seller: '',
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
      type: Number,
      default: 0
    }
  },

  methods: {
    save() {
      const lastQuoteAsNumber = Number(this.lastQuoteNumber);
      this.quote.quote = String(lastQuoteAsNumber + 1).padStart(4, '0');
      this.$emit('save', this.quote);
      this.quote  = this.defaultReset;

    },

    close() {
      this.$emit('cancel');
    }
  }
}
</script>