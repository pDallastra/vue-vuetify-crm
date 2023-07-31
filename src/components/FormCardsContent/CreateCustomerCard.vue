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
              v-model="quote.amoutItems"
              label="Amount of Items"
              type="number"
              min="0"
            />
          </v-col>
        </v-row>

        <div v-if="quote.amoutItems.length">
          <div class="py-2">Items</div>
          <v-row v-for="(item, index) in Number(quote.amoutItems)" :key="index">
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
    quote: {
      quote: "",
      customer: "",
      amoutItems: 0,
      price: 0,
      seller: "",
      itemList: [
        {
          width: 0,
          height: 0,
        },
      ],
    },
    defaultReset: {
      quote: "",
      customer: "",
      amoutItems: 0,
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
    "quote.amoutItems": {
      handler(newValue, oldValue) {
        if (newValue > 0) {
          if (newValue > oldValue && oldValue !== 0) {
            if (!this.quote.itemList[newValue])
              this.quote.itemList.push({
                width: 0,
                height: 0,
              });
          }
          if (oldValue > newValue && !this.closingModal) {
            this.quote.itemList[newValue] = {
              width: 0,
              height: 0,
            };
          }

          console.log(this.quote);
        }
      },
    },
  },

  methods: {
    save() {
      const lastQuoteAsNumber = Number(this.lastQuoteNumber);
      this.quote.quote = String(lastQuoteAsNumber + 1).padStart(4, "0");
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
