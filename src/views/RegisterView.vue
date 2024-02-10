<template>
  <div class="register">
    <h2>Регистрация</h2>
    <Register/>
  </div>
</template>

<script>
// @ is an alias to /src
import Register from '@/components/Register.vue'

export default {
  name: 'RegisterView',
  components: {
    Register
  }
}
</script>