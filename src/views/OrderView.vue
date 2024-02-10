<template>
  <div class="order">
    <h2>Заказы</h2>
    <Order/>
  </div>
</template>

<script>
// @ is an alias to /src
import Order from '@/components/Order.vue'

export default {
  name: 'OrderView',
  components: {
    Order
  }
}
</script>