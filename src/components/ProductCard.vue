<template>
  <b-card
    :img-src="product.img || '@/assets/logo.png'"
    img-alt="Image"
    img-top
    img-height="260"
    style="width: 20rem"
    class="mb-2"
    @click="goTo"
  >
    <b-card-title class="text-center" v-text="product.name || 'producto'" />
    <b-card-text class="text-center">
      Disponibles: {{ product.available }}
    </b-card-text>
    <b-card-text class="text-center text-black">
      <b> $ {{ product.price | clp }} </b>
    </b-card-text>
    <b-card-text>
      <b-input-group class="d-flex justify-content-center">
        <b-input-group-prepend>
          <b-button variant="outline-secondary" @click.stop="removeQty()">
            -
          </b-button>
        </b-input-group-prepend>
        <b-form-input
          variant="outline-secondary"
          v-model="qty"
          type="number"
          class="text-center"
          style="max-width: 3rem"
          :max="+product.available"
          @blur.stop="checkQty"
          @click.stop
        />
        <b-input-group-append>
          <b-button
            variant="outline-secondary"
            @click.stop="addQty(product.available)"
          >
            +
          </b-button>
        </b-input-group-append>
      </b-input-group>
    </b-card-text>
    <b-card-text class="text-center">
      <b-button
        href="#"
        variant="outline-secondary"
        @click.stop="addCart(product)"
        :disabled="product.available === 0"
      >
        Agregar al Carro
      </b-button>
    </b-card-text>
  </b-card>
</template>

<script>
export default {
  name: "ProductCard",
  props: {
    product: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      qty: this.product.available === 0 ? 0 : 1,
    };
  },
  watch: {
    "product.available"() {
      if (this.product.available === 0) {
        this.qty = 0;
      } else {
        this.qty = 1;
      }
    },
  },
  methods: {
    goTo() {
      this.$router.push({
        name: "producto",
        params: { productId: this.product.id },
      });
    },
    addCart(product) {
      const item = { ...product, qty: this.qty };
      this.$emit("add-cart", item);
    },
    addQty(available) {
      if (this.qty >= available) return;
      this.qty += 1;
    },
    removeQty() {
      if (this.qty === 1) return;
      this.qty -= 1;
    },
    checkQty() {
      if (+this.qty > +this.product.available) {
        this.qty = +this.product.available;
      }
      if (+this.qty < 0) {
        this.qty = 0;
      }
    },
  },
};
</script>
<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>
