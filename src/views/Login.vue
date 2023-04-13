<template>
  <div id="login-box" class="ui segment">
    <div class="ui teal segment">
      <div class="ui action input fluid">
        <a class="ui teal ribbon label">Login</a>
      </div>
    </div>
    <form class="ui form" @submit.prevent="login">
      <div class="field">
        <label for="email">User Account:</label>
        <input
          type="text"
          placeholder="username"
          v-model="User.username"
          required
        />
      </div>
      <div class="field">
        <label for="password">Password:</label>
        <input
          type="password"
          placeholder="password"
          v-model="User.password"
          required
        />
      </div>
      <button class="ui button" type="submit" @click="loginAPI">Login</button>
    </form>
  </div>
  <div v-if="isLoading">
    <div class="ui active inverted dimmer">
      <div class="ui text loader">Loading</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import navbar from "../components/navbar.vue";
import { useCookies } from "vue3-cookies";
export default {
  name: "login",
  setup() {
    const { cookies } = useCookies();
    return { cookies };
  },
  data() {
    return {
      isLoading: false,
      User: {
        username: "",
        password: "",
      },
    };
  },
  methods: {
    loginAPI() {
      this.isLoading = true;
      console.log("calling loginAPI");
      console.log(this.isLoading);
      const url = "https://as-backen-1474.onrender.com/login";
      axios
        .post(url, this.User)
        .then((r) => {
          if (r.data.token) {
            axios.defaults.headers.common["Authorization"] =
              "Bearer " + r.data.token;
            this.$cookies.set("token", r.data.token, 600);
            alert("Login Successfull");
            this.$router.replace("/contacts");
          } else {
            alert(r.data.message);
          }
        })
        .catch((error) => {
          alert(error);
        });
      setTimeout(() => {
        this.isLoading = false;
      }, 2000);
    },
  },
  components: {
    navbar,
  },
};
</script>

<style>
#login-box {
  width: 420px;
  margin: auto;
}
</style>
