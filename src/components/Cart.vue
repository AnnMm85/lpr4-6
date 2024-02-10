<template>
  <button class="btn" @click="toHomePage" style="margin-right: 80%;">Назад</button>
  <div class="cart">
    <div v-if="cart.length === 0">
      <p>Корзина пуста</p>
    </div>
    <div  v-else class="cart-container">
      <div class="product-box" v-for="product in cartPr " :key="product.id">
        <div class="product-name"> Название: {{ product.name }}</div>
        <div class="product-desc"> Описание: {{ product.description }}</div>
        <div class="prod-footer">

          <button v-on:click="product.quantity -= 1">-</button>
          <div class="countAdd">
            <div class="product-quantity"> Количество: {{ product.quantity }}</div>
          </div>
          <button v-on:click="product.quantity += 1">+</button>

          <div class="product-price"> Цена: {{ product.price }}</div>
        </div>
        <button @click="deleteProduct(product)" type="submit" class="btn">Удалить из корзины</button>
      </div>
    </div>
  </div>
  <button @click="userOrders" v-if="cart.length > 0" class="btn btn-ord" style="background-color: rgb(197, 197, 255);">
    Оформить заказ
  </button>
  <div class="popup" v-if="showPopup">
    Товар успешно удален
  </div>
</template>


<script>
export default {
  name: 'Cart',
  data() {
    return {
      url: "https://jurapro.bhuser.ru/api-shop",
      cart: [],
      cartPr: {},
      showPopup: false,
    };
  },
  created() {
    this.getData();
  },
  methods: {
    async getData() {
      const localToken = localStorage.getItem('userToken');
      const response = await fetch(this.url + '/cart', {
        method: 'GET',
        headers: {
          'Content-type': 'application/json',
          'Authorization': `Bearer ${localToken} `
        },
      })
      if (response.ok) {
        const result = await response.json();
        this.cart = result.data;
        this.cart.forEach(el => {
          if (!this.cartPr[el.product_id]) {
            console.log(el)
            this.cartPr[el.product_id]= {id: el.id, name: el.name, description: el.description, price:el.price}
            this.cartPr[el.product_id].quantity = 1
          } else {
            this.cartPr[el.product_id].quantity ++
          }
        })
      } else {
        this.error = "Ошибка";
        console.error(this.error);
      }
    },
    toHomePage() {
      this.$router.push('/');
    },

    async deleteProduct(product) {
      const token = localStorage.getItem('userToken');
      if (!token) {
        console.error('Токен пользователя отсутствует.');
        return;
      }

      try {
        const response = await fetch(`${this.url}/cart/${product.id}`, {
          method: "DELETE",
          headers: {
            "Authorization": `Bearer ${token}`
          }
        });
        if (response.ok) {
          console.log("Товар успешно удален из корзины");
          this.showPopup = true;
          setTimeout(() => {
            this.showPopup = false;
          }, 1000);
          setTimeout(() => {
            location.reload()
          }, 1000)

        } else {
          console.error("Ошибка удаления товара из корзины:", response.statusText);
        }
      } catch (error) {
        console.error("Ошибка удаления товара из корзины:", error);
      }
    },
    async userOrders() {
      const token = localStorage.getItem('userToken');
      if (!token) {
        console.error('Токен пользователя отсутствует.');
        return;
      }

      const url = "https://jurapro.bhuser.ru/api-shop/order";
      try {
        const response = await fetch(url, {
          method: "POST",
          headers: {
            "Authorization": `Bearer ${token}`
          }
        });

        if (response.ok) {
          const data = await response.json();
          console.log(data.data.message);
          this.$router.push('/order');
        } else if (response.status === 422) {
          const errorData = await response.json();
          console.error("Ошибка оформления заказа:", errorData.error.message);
        } else {
          console.error("Ошибка оформления заказа:", response.statusText);
        }
      } catch (error) {
        console.error("Ошибка оформления заказа:", error);
      }
    },
  }
};
</script>