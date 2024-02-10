<template>
  <div class="login">
    <h2>Вход</h2>
    <Login/>
  </div>
</template>

<script>
// @ is an alias to /src
import Login from '@/components/Login.vue'

export default {
  name: 'LoginView',
  components: {
    Login
  }
}
</script>