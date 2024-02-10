<template>
  <button @click="toHomePage" class="btn" style="margin-right: 80%;">Назад</button>
  <div class="user-orders">
    <div v-if="orders.length === 0">
      <p>У вас пока нет оформленных заказов.</p>
    </div>
    <div v-for="order in orders" v-else :key="order.id" class="order-item">
      <h3>Заказ №{{ order.id }}</h3>
      <p>Список товаров: {{ order.products.join(', ') }}</p>
      <p>Общая стоимость заказа: {{ order.order_price }}</p>
    </div>
  </div>

</template>

<script>
export default {
  data() {
    return {
      url: 'https://jurapro.bhuser.ru/api-shop',
      orders: []
    };
  },
  created() {
    this.Order();
  },
  methods: {
    async Order() {
      const token = localStorage.getItem('userToken');
      if (!token) {
        console.error('Токен пользователя отсутствует.');
        return;
      }
      try {
        const response = await fetch(this.url + '/order', {
          headers: {
            "Authorization": `Bearer ${token}`
          }
        });
        if (response.ok) {
          const data = await response.json();
          this.orders = data.data;
        } else {
          console.error("Ошибка получения списка оформленных заказов");
        }
      } catch (error) {
        console.error("Ошибка загрузки данных:", error);
      }
    },
    toHomePage() {
      this.$router.push('/');
    }
  }
};
</script>

<style scoped>
.user-orders {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  padding: 20px;
  gap: 15px;
}

.order-item {
  border: 1px solid #dee2e6;
  border-radius: 5px;
  padding: 20px;
  width: calc(30% - 40px);
  text-align: center;
  box-shadow: 5px 5px 5px #e7e7e7;
  background-color: #99ace2;
}

.order-item h3 {
  margin-bottom: 10px;
}

.back-button {
  padding: 10px;
  background-color: #6c757d;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  display: flex;
  margin: 20px auto;
  width: 100px;
  justify-content: center;
}
.back-button:hover{
  background-color: #4f575d;
}

h1{
  text-align: center;
}
</style>