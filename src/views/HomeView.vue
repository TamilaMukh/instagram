<template>
  <div class="home">
    <div class="container mx-auto">
      <div class="w-1/2 mx-auto my-8" v-for="post of friendsPost" :key="post.id">
        <img class="w-full" :src="post.image" alt="" />
      </div>
    </div> 
    <div class="fixed right-5 top-40">
      <h1 class="font-semibold text-lg mb-3">Все пользователи</h1>
      <div class="my-2 flex items-center justify-between" v-for="user of allUsers" :key="user.id">
        <div class="flex items-center mr-3">
          <img class="w-7 h-7 mr-2" :src="user.profile.avatar" alt="">
          <p>{{ user.surname + '' + user.name }}</p>
        </div>
        <p @click="addFriend(user.email)">+</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HomeView",
  data() {
    return {
      users: null,
      posts: null,
      currentUser: localStorage.getItem("loggedUser"),
      showModal: 0,
      singlePost: null,
      newPost: {
        user_id: null,
        image: null,
        description: null,
        likes: [],
        comments: [],
      },
    };
  },
  computed: {
    filteredUsers() {
      if (this.users != null) {
        return this.users.filter(
          (e) => e.email == localStorage.getItem("loggedUser")
        );
      } else {
        return console.log("hello");
      }
    },
    filteredPosts() {
      if (this.posts != null) {
        return this.posts.filter((e) => e.user_id == this.filteredUsers[0].id);
      } else {
        return console.log("hello");
      }
    },
    friendsPost() {
      if (this.posts) {
        let arr = this.posts.filter((e) =>
          this.filteredUsers[0].subscriptions.includes(e.user_email)
        );
        return arr;
      }
      return [];
    },
    allUsers() {
      if(this.users) {
        let arr = this.users.filter((i) => !this.filteredUsers[0].subscriptions.includes(i.email))
        return arr      
      }
      return [];
    }
  },
  methods: {
    async submitPost() {
      await axios.post(
        "https://6286235096bccbf32d6fe5bf.mockapi.io/posts",
        this.newPost
      );
      this.$router.go();
    },
    openModal(item) {
      this.singlePost = item;
      this.showModal = 1;
    },
    async addLike(id) {
      this.showModal = 0;
      let currentPost = this.filteredPosts[id];
      if (currentPost.likes.includes(localStorage.getItem("loggedUser"))) {
        let postIndex = currentPost.likes.indexOf(
          localStorage.getItem("loggedUser")
        );
        currentPost.likes.splice(postIndex, 1);
        await axios.put(
          "https://6286235096bccbf32d6fe5bf.mockapi.io/posts/" + currentPost.id,
          currentPost
        );
      } else {
        currentPost.likes.push(localStorage.getItem("loggedUser"));
        await axios.put(
          "https://6286235096bccbf32d6fe5bf.mockapi.io/posts/" + currentPost.id,
          currentPost
        );
      }
    },
    async addFriend(email) {
      let a = this.filteredUsers[0].subscriptions.push(email)
      await axios.put("https://6286235096bccbf32d6fe5bf.mockapi.io/users/" + this.filteredUsers[0].id, a)
    }
  },
  async mounted() {
    this.users = (
      await axios.get("https://6286235096bccbf32d6fe5bf.mockapi.io/users")
    ).data;
    this.newPost.user_id = parseInt(this.filteredUsers[0].id);

    this.posts = (
      await axios.get("https://6286235096bccbf32d6fe5bf.mockapi.io/posts")
    ).data;
  },
};
</script>