<template>
  <div class="container mx-auto">
    <div class="flex items-center my-10 border-b pb-10">
      <img
        class="w-28 h-28 object-cover mr-5 rounded-full"
        :src="filteredUsers[0].profile.avatar"
        alt=""
      />
      <div>
        <p class="text-2xl font-light mb-5">{{ filteredUsers[0].nickname }}</p>
        <div class="flex items-center mb-5">
          <p class="mr-4"><span class="font-bold">{{ filteredPosts.length }}</span> публикаций</p>
          <p class="mr-4">
            <span class="font-bold">{{
              filteredUsers[0].followers.length
            }}</span>
            подписчиков
          </p>
          <p class="mr-4">
            <span class="font-bold">{{
              filteredUsers[0].subscriptions.length
            }}</span>
            подписок
          </p>
        </div>
        <div>
          <p class="font-semibold">
            {{ filteredUsers[0].name + " " + filteredUsers[0].surname }}
          </p>
          <p>{{ filteredUsers[0].profile.description }}</p>
        </div>
      </div>
    </div>
    <div class="flex mb-5 justify-between">
      <input
        v-model="newPost.image"
        class="w-1/3 p-2 rounded-lg border"
        placeholder="Введите ссылку на картинку"
        type="text"
      />
      <input
        v-model="newPost.description"
        class="w-1/3 p-2 rounded-lg border"
        placeholder="Введите описание"
        type="text"
      />
      <button @click="submitPost()" class="p-2 rounded-lg bg-main text-white">
        Опубликовать
      </button>
    </div>
    <div class="flex flex-wrap flex-row-reverse justify-between w-full">
      <div
        class="w-third relative post my-3"
        v-for="(post, index) of filteredPosts"
        :key="post.id"
        @click="openModal(post)"
      >
        <img class="h-48 w-full object-cover" :src="post.image" alt="" />
        <div class="likes">
          <p
            @click="addLike(index)"
            :class="{ 'text-red-500': post.likes.includes(currentUser) }"
            class="text-white mx-2"
          >
            <i class="fa-regular fa-heart"></i> {{ post.likes.length }}
          </p>
          <p class="text-white mx-2">
            <i class="fa-regular fa-comment"></i> {{ post.comments.length }}
          </p>
        </div>
      </div>
    </div>
    <div v-if="showModal === 1" class="fixed top-0 left-0 w-full h-full bg-black bg-opacity-30">
      <div class="relative w-full h-full">
        <div @click="showModal = 0" class="absolute top-5 right-5 hover:cursor-pointer">
          <p class="text-3xl text-white"><i class="fa-solid fa-xmark"></i></p>
        </div>
        <div class="abs bg-white w-4/5 h-4/5">
          <div class="flex h-full">
            <div class="bg-black w-3/5 h-full">
              <img  class="w-full h-full object-contain" :src="singlePost.image" alt="" />
            </div>
            <div class="w-2/5 flex flex-col h-full justify-between">
              <div class="flex p-3 items-center border-b w-full">
                <img
                  class="w-10 h-10 object-cover mr-5"
                  :src="filteredUsers[0].profile.avatar"
                  alt=""
                />
                <p>{{ filteredUsers[0].nickname }}</p>
              </div>
              <div class="px-3" v-for="comment of singlePost.comments" :key="comment.id">
                <p class="">{{ comment.text }}</p>
              </div>
              <div class="flex">
                <input class="border-t w-full py-3 px-5" placeholder="Создай коммент" type="text">
                <button class="text-white font-semibold p-2 bg-main">Отправить</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "ProfileView",
  data() {
    return {
      users: null,
      posts: null,
      currentUser: localStorage.getItem("loggedUser"),
      showModal: 0,
      singlePost: null,
      newPost: {
        user_id: null,
        user_email: localStorage.getItem("loggedUser"),
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
      this.singlePost = item
      this.showModal = 1
    },
    async addLike(id) {
      this.showModal = 0
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