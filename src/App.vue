<template>
  <div id="app">
    <RegistrationLogin v-if="currentUser == null"/>
    <div v-else>
      <HeaderSet />
      <router-view/>
    </div>
  </div>
</template>

<script>
import RegistrationLogin from './views/RegistrationLogin.vue'
import HeaderSet from './components/HeaderSet.vue'
export default {
  components: {
    RegistrationLogin,
    HeaderSet
  },
  data() {
    return {
      currentUser: localStorage.getItem('loggedUser')
    }
  }
}
</script>