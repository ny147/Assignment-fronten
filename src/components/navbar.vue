<script>
import { useCookies } from "vue3-cookies";
import { useRouter } from "vue-router";
const router = useRouter();
export default {
  setup() {
    const { cookies } = useCookies();
    return { cookies };
  },
  data() {
    return {
      isLoggedIn: "",
    };
  },
  mounted() {},
  methods: {
    handleSignout() {
      this.$cookies.remove("token");
      this.$router.push("/login");
    },
    checkstatus() {},
  },
  watch: {
    $route(to, from) {
      if (this.$cookies.isKey("token")) {
        this.isLoggedIn = true;
      } else {
        this.isLoggedIn = false;
      }
    },
  },
};
</script>

<template>
  <!-- <div class="ui inverted menu">
    <a class="ui Tiny image">
      <img src="https://www.vectorlogo.zone/logos/vuejs/vuejs-icon.svg" />
    </a>

    <a class="item">
      <router-link to="/contacts">
        <button class="ui black button">Contact</button>
      </router-link>
    </a>

    <div class="right menu">
      <a class="item" v-if="!isLoggedIn">
        <router-link to="/login">
          <button class="ui black button">
            <i class="circular users icon"></i>
          </button>
        </router-link>
      </a>
      <a class="item" v-else>
        <button class="ui red button" @click="handleSignout">sign out</button>
      </a>
    </div>
  </div> -->
  <div class="ui top menu inverted">
    <div class="item">
      <img src="../assets/logo.svg" />
    </div>
    <a class="item">Contacts</a>
    <a class="item right" v-if="!isLoggedIn">
      <router-link to="/login">
        <i class="big user icon menu inverted icon-user"> </i>
      </router-link>
    </a>
    <a class="item right" v-else>
      <i class="big sign out alternate icon" @click="handleSignout"></i>
    </a>
  </div>
</template>

<style></style>
