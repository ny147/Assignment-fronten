<script>
import axios from "axios";
import { useCookies } from "vue3-cookies";
export default {
  name: "Users",
  setup() {
    const { cookies } = useCookies();
    return { cookies };
  },
  data() {
    return {
      isLoading: false,
      Users: [],
      search: "",
      uid: "",
    };
  },
  mounted() {
    this.isLoading = true;
    var url = "https://as-backen-1474.onrender.com/contacts/";
    const JWT_token = {
      headers: {
        Authorization: "Bearer " + this.$cookies.get("token"),
      },
    };
    axios.get(url, JWT_token).then((response) => {
      console.log(response.data);
      this.Users = response.data;
    });
    setTimeout(() => {
      this.isLoading = false;
    }, 2000);
  },
  computed: {
    filterUsers: function () {
      return this.Users.filter((user) => {
        return (
          user.firstname.match(this.search) || user.lastname.match(this.search)
        );
      });
    },
  },
  methods: {
    DeleteUser(contactId) {
      if (confirm("are you sure to delete ")) {
        const JWT_token = {
          headers: {
            Authorization: `Bearer ${this.$cookies.get("token")}`,
          },
        };
        var url = "https://as-backen-1474.onrender.com/contacts/" + contactId;
        axios
          .delete(url, JWT_token)
          .then(() => {
            console.log("Delete user id: " + contactId);
            window.location.reload();
          })
          .catch((error) => {
            console.error();
            alert("what");
          });
      }
    },
  },
};
</script>
<template>
  <div id="search" class="ui teal segment">
    <div class="ui action input fluid">
      <a class="ui teal ribbon label">Contact</a>
      <input v-model="search" type="text" placeholder="Search..." />
      <button class="ui button">Search</button>
      <router-link to="/addcontact">
        <button class="ui orange button Add-btn">+ Add</button>
      </router-link>
    </div>
  </div>
  <div id="card-box" class="ui segment link cards">
    <div class="ui card" v-for="auser in filterUsers" v-bind:key="auser.id">
      <div class="image">
        <img :src="auser.imageUrl" />
      </div>
      <div class="content">
        <div class="header">{{ auser.firstname }} {{ auser.lastname }}</div>

        <div class="description">
          <div>Mobile: {{ auser.mobile }}</div>
          <div>Email: {{ auser.email }}</div>
          <div>Facebook: {{ auser.facebook }}</div>
        </div>
      </div>

      <div class="extra content">
        <router-link
          :to="{
            path: 'editcontact',
            name: 'editcontact',
            params: { contactId: auser._id },
          }"
        >
          <button class="ui blue icon button">
            <i class="edit icon"></i>
          </button>
        </router-link>
        <button class="ui red icon button">
          <i class="trash alternate icon" @click="DeleteUser(auser._id)"></i>
        </button>
      </div>
    </div>
  </div>
  <div class="loader-active" v-if="isLoading">
    <div class="ui active inverted dimmer">
      <div class="ui text loader">Loading</div>
    </div>
  </div>
</template>

<style scoped>
#search {
  margin: 20px;
}
#card-box {
  margin: 20px;
  align-items: center;
  justify-content: center;
}
.content {
  align-items: center;
  justify-content: center;
  text-align: center;
}
.loader-active {
  position: relative;
}
.loader-active .dimmer {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex !important;
  justify-content: center;
  align-items: center;
}
</style>
