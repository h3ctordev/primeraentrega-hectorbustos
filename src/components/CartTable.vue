<template>
  <b-container>
    <b-row>
      <b-table :items="cart" :fields="fields">
        <template #cell(price)="{ item }"> $ {{ item.price | clp }} </template>
        <template #cell(total)="{ item }"> $ {{ item.total | clp }} </template>
        <template #custom-foot>
          <b-tr>
            <b-th></b-th>
            <b-th></b-th>
            <b-th class="text-end">Total</b-th>
            <b-th class="text-center"> $ {{ getTotal | clp }}</b-th>
          </b-tr>
        </template>
      </b-table>
    </b-row>
    <b-row align-h="end">
      <b-button class="mx-1" variant="outline-danger" @click="onCartClean">
        Limpiar
      </b-button>
      <b-button
        variant="success"
        :disabled="countItems === 0"
        @click="onConfirmBuyout"
      >
        Comprar
      </b-button>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "CartTable",
  props: {
    cart: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    countItems() {
      return this.cart.reduce((sum, a) => (sum += a.qty), 0);
    },
    getTotal() {
      return this.cart.reduce((sum, a) => (sum += a.total), 0);
    },
  },
  data() {
    return {
      fields: [
        { key: "name", label: "Nombre", class: "text-center" },
        {
          key: "price",
          label: "Precio",
          class: "text-center",
        },
        { key: "qty", label: "Cantidad", class: "text-center" },
        { key: "total", label: "Total", class: "text-center" },
      ],
    };
  },
  methods: {
    onConfirmBuyout() {
      this.open = false;
      this.$emit("complete", this.getTotal);
    },
    onCartClean() {
      this.$emit("cart-clean");
    },
  },
  filters: {
    clp: (value) => {
      return new Intl.NumberFormat("es-CL", {
        maximumSignificantDigits: 5,
      }).format(value);
    },
  },
};
</script>
