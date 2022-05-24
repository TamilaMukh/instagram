<template>
  <div class="py-5 bg-white">
    <div class="container mx-auto">
      <div class="flex items-center justify-between">
        <h1 class="text-center text-5xl font-bold">SingleRoma</h1>
        <div>
          <input
            class="border p-2 rounded-lg"
            type="text"
            placeholder="Поиск"
          />
        </div>
        <div class="text-2xl w-1/6 flex items-center justify-between">
          <router-link to="/">
            <i class="fa-solid fa-house"></i>
          </router-link>
          <router-link to="/">
            <i class="fa-brands fa-facebook-messenger"></i>
          </router-link>
          <router-link to="/">
            <i class="fa-regular fa-square-plus"></i>
          </router-link>
          <router-link to="/">
            <i class="fa-regular fa-compass"></i>
          </router-link>
          <router-link to="/">
            <i class="fa-regular fa-heart"></i>
          </router-link>
          <router-link to="/profile">
            <i class="fa-regular fa-user"></i>
          </router-link>
        </div>
        <button @click="logout()" class="bg-red-500 p-2 text-white rounded-lg">Выйти</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HeaderSet",
  methods: {
    logout() {
      localStorage.removeItem('loggedUser')
      this.$router.go()
    }
  }
};
</script>