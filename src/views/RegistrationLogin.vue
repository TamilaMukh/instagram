<template>
  <div class="bg-slate-100 py-52">
    <div class="container mx-auto">
      <div class="flex items-center justify-center">
        <div class="w-1/2">
          <img class="w-1/2 float-right" src="https://www.instagram.com/static/images/homepage/phones/home-phones-2x.png/cbc7174b4f05.png" alt="">
        </div>
        <div class="w-1/2">
          <div class="w-10/12 bg-white p-10 rounded-xl">
            <h1 class="text-center text-5xl font-bold mb-5">SingleRoma</h1>
            <div v-if="regLog === 1">
              <input v-model="login.email" class="border rounded-lg p-2 w-full my-3" placeholder="Электронная почта" type="text">
              <input v-model="login.password" class="border rounded-lg p-2 w-full my-3" placeholder="Пароль" type="text">
              <button @click="loginUser()" class="bg-main text-white font-bold rounded-lg w-1/2 block p-2 mt-4 mx-auto">Войти</button>
            </div>
            <div v-if="regLog === 2">
              <input v-model="reg.email" class="border rounded-lg p-2 w-full my-3" placeholder="Электронная почта" type="text">
              <input v-model="reg.phone" class="border rounded-lg p-2 w-full my-3" placeholder="Мобильный телефон" type="text">
              <input v-model="reg.name" class="border rounded-lg p-2 w-full my-3" placeholder="Имя" type="text">
              <input v-model="reg.surname" class="border rounded-lg p-2 w-full my-3" placeholder="Фамилия" type="text">
              <input v-model="reg.nickname" class="border rounded-lg p-2 w-full my-3" placeholder="Имя пользователя" type="text">
              <input v-model="reg.password" class="border rounded-lg p-2 w-full my-3" placeholder="Пароль" type="text">
              <button @click="registerUser()" class="bg-green-500 text-white font-bold rounded-lg w-1/2 block p-2 mt-4 mx-auto">Зарегистрироваться</button>
            </div>
            <p class="my-3 text-sm font-semibold">Если вы не зарегистрированы, то Рома поможет вам зарегистрироваться нажав на кнопку ниже</p>
            <button v-if="regLog === 1" @click="regLog = 2" class="bg-green-500 text-white font-bold rounded-lg w-1/2 block p-2 mt-4 mx-auto">Зарегистрироваться</button>
            <button v-else @click="regLog = 1" class="bg-main text-white font-bold rounded-lg w-1/2 block p-2 mt-4 mx-auto">Войти</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "RegistrationLogin",
  data() {
    return {
      users: null,
      regLog: 1,
      reg: {
        email: null,
        phone: null,
        name: null,
        surname: null,
        nickname: null,
        password: null,
        profile: {
          avatar: 'https://i.pinimg.com/originals/ff/a0/9a/ffa09aec412db3f54deadf1b3781de2a.png',
          description: '...'
        },
        posts: [],
        followers: [],
        subscriptions: []
      },
      login: {
        email: null,
        password: null
      },
      currentUser: localStorage.getItem('loggedUser')
    }
  },
  methods: {
    async registerUser() {
      await axios.post('https://6286235096bccbf32d6fe5bf.mockapi.io/users', this.reg)
      this.$router.go()
    },
    loginUser() {
      this.users.forEach(item => {
        if(item.email == this.login.email && item.password == this.login.password) {
          localStorage.setItem('loggedUser', item.email)
          this.$router.go()
        } else {
          console.log('неудачно')
        }
      });
    }
  },
  async mounted() {
    this.users = (await axios.get('https://6286235096bccbf32d6fe5bf.mockapi.io/users')).data
  }
}
</script>