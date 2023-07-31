<template>
  <v-card>
    <v-card-title>
      <span class="text-h5">{{ formTitle }}</span>
    </v-card-title>

    <v-card-text>
      <v-container>
        <v-row>
          <v-col cols="12" sm="12" md="12">
            <v-select
              v-model="quote.customer"
              :items="customers"
              density="customer"
              label="Customer"
            ></v-select>
          </v-col>
          <v-col cols="12" sm="6" md="6">
            <v-select
              v-model="quote.seller"
              :items="sellers"
              density="seller"
              label="Seller"
            ></v-select>
          </v-col>
          <v-col cols="12" sm="6" md="6">
            <v-text-field
              v-model="quote.amountItems"
              label="Amount of Items"
              type="number"
              min="0"
            />
          </v-col>
        </v-row>

        <div v-if="quote.amountItems.length">
          <v-col 
            v-for="(item, index) in Number(quote.amountItems)" 
            :key="index"
          >
          <div class="py-2">Item {{ item }}</div>
            <v-row>
              <v-col cols="12" sm="12" md="12">
                <v-text-field
                  v-model="quote.itemList[index].name"
                  label="Product Name"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="quote.itemList[index].width"
                  label="Width"
                  type="number"
                />
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="quote.itemList[index].height"
                  label="Height"
                  type="number"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="quote.itemList[index].quantity"
                  label="Quantity"
                  type="number"
                />
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-select
                  v-model="quote.itemList[index].substrate"
                  :items="substrateList"
                  density="Substrate"
                  label="Substrate"
                ></v-select>
              </v-col>
            </v-row>
          </v-col>
        </div>
      </v-container>
    </v-card-text>

    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn color="blue darken-1" text @click="close"> Cancel </v-btn>
      <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "CreateCustomerCard",

  data: () => ({
    closingModal: false,
    customers: [
      { text: "Apple", id: 1 },
      { text: "Microsoft", id: 2 },
      { text: "Samsung", id: 3 },
    ],
    sellers: [
      { text: "Rogério Souza", id: 1 },
      { text: "Fernanda Motta", id: 2 },
      { text: "Paulo Dallastra", id: 3 },
    ],
    substrateList: [
      { text: 'BOPP Metalizado', id: 1},
      { text: 'BOPP Branco', id: 2},
      { text: 'Couchê 20g', id: 3}
    ],
    quote: {
      quote: "",
      customer: "",
      amountItems: 0,
      price: 0,
      seller: "",
      itemList: [
        {
          name: '',
          width: 0,
          height: 0,
          quantity: 0,
          substrate: ''
        },
      ],
    },
    defaultReset: {
      quote: "",
      customer: "",
      amountItems: 0,
      price: 0,
      seller: "",
    },
  }),

  props: {
    formTitle: {
      type: String,
      default: "",
    },

    lastQuoteNumber: {
      type: [Number, String],
      default: 0,
    },
  },

  watch: {
    "quote.amountItems": {
      handler(newValue, oldValue) {
        if (newValue > 0) {
          if (newValue > oldValue && oldValue !== 0) {
            if (!this.quote.itemList[newValue])
              this.quote.itemList.push({
                name: '',
                width: 0,
                height: 0,
                quantity: 0,
                substrate: ''
              });
          }
          if (oldValue > newValue && !this.closingModal) {
            this.quote.itemList[newValue] = {
              name: '',
              width: 0,
              height: 0,
              quantity: 0,
              substrate: ''
            };
          }

          console.log(this.quote);
        }
      },
    },
  },

  methods: {
    calcItemPrice(item) {
      return ((item.width * item.height) / 1000) * (item.quantity / 10);
    },

    save() {
      const lastQuoteAsNumber = Number(this.lastQuoteNumber);
      this.quote.quote = String(lastQuoteAsNumber + 1).padStart(4, "0");
      for (let item of this.quote.itemList) {
        item.price = this.calcItemPrice(item);
        this.quote.price += item.price;
      }
      this.$emit("save", this.quote);
      this.closingModal = true;
      this.quote = this.defaultReset;
    },

    close() {
      this.closingModal = true;
      this.quote = this.defaultReset;
      this.$emit("cancel");
    },
  },
};
</script>
