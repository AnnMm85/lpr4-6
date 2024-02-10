<template>
  <nav>
    <div class="navDiv">
      <div>
        <router-link to="/">Home</router-link>
        |
      </div>
      <div v-if="!isAuthenticated">
        <router-link to="/login">Login</router-link>
        |
        <router-link to="/register">Register</router-link>
        |
      </div>
      <div v-else>
        <router-link to="/cart">Cart</router-link>
        |
        <router-link to="/order">Оформленные заказы</router-link>
        |
        <a href="#" @click="logout">Выход</a>
      </div>
    </div>

  </nav>
  <router-view/>
</template>

<script>
export default {
  data() {
    return {
      url: "https://jurapro.bhuser.ru/api-shop",
    }
  },
  methods: {
    logout() {
      localStorage.removeItem('userToken');
      this.$router.push('/');
      location.reload()
    }
  },
  computed: {
    isAuthenticated() {
      return !!localStorage.getItem('userToken');
    }
  }

}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
<script setup>
</script>