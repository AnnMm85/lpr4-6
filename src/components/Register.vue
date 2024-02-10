<template>
  <form class="register" @submit.prevent="register">
    <label>ФИО</label>
    <input type="text" required v-model="fio">
    <label>Почта</label>
    <input type="email" required v-model="email">
    <label>Пароль</label>
    <input type="password" required v-model="password">
    <button type="submit" >Send</button>
    <hr/>
  </form>
</template>


<script>
export default {
  name: "Register",
  data() {
    return {
      url: "https://jurapro.bhuser.ru/api-shop",
      fio: "",
      email: "",
      password: "",
    };
  },
  methods: {
    async register() {
      const user = {
        fio: this.fio,
        email: this.email,
        password: this.password,
      }

      const response = await fetch(this.url + '/signup',{
        method:'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(user)
      })
      if (response.ok) {
        this.$router.push('/login');
      } else {
        this.error = "Ошибка при регистрации";
        console.error('Ошибка:', this.error);
      }
    },
  }
};
</script>
