<template>
  <a href="#" @click="getCatalogProduct" class="btn" style="
    width: 320px;"> Показать каталог</a>
  <div class="catalog">
    <div>
      <div class="product-box" v-for="product in products" :key="product.id">
        <div class="product-name"> Название: {{ product.name }}</div>
        <div class="product-desc"> Описание: {{ product.description }}</div>
        <div class="prod-footer">
          <div class="product-price"> Цена: {{ product.price }}</div>
          <button v-on:click="addToCart(product)" class="btn"> В корзину</button>
        </div>
      </div>
    </div>
  </div>
  <div class="popup" v-if="showPopup">
    Товар добавлен в корзину
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      url: "https://jurapro.bhuser.ru/api-shop",
      products: [],
      productsInCart: [],
      showPopup: false
    }
  },
  methods: {
    async getCatalogProduct() {
      const response = await fetch(this.url + '/products', {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
        },
      });
      if (response.ok) {
        const result = await response.json();
        this.products = result.data
        console.log('Result: ', result)
      } else {
        this.error = "Ошибка";
        console.error(this.error);
      }
    },

    logout() {
      localStorage.removeItem('userToken');
      this.$router.push('/');
      location.reload()
    },

    async addToCart(product) {
      const response = await fetch(`${this.url}/cart/${product.id}`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${localStorage.getItem('userToken')}`
        },
      });
      if (response.ok) {
        const result = await response.json();
        console.log('Result: ', result)
        this.productsInCart.push(product);
        this.showPopup = true;
        setTimeout(() => {
          this.showPopup = false;
        }, 1000);
      } else {
        this.error = "Ошибка при добавлении товара в корзину";
        console.error(this.error);
      }
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