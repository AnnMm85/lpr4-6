<template>
  <div class="cart">
    <h2>Корзина товаров</h2>
    <Cart />
  </div>
</template>

<script>
// @ is an alias to /src
import Cart from '@/components/Cart.vue'

export default {
  name: 'CartView',
  components: {
    Cart
  }
}
</script>