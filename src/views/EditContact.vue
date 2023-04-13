<template>
  <div id="form-box" class="ui segment">
    <h2 class="ui header">
      Contact
      <a class="ui teal label">Add</a>
    </h2>
    <div class="ui divider"></div>
    <form @submit.prevent="editContact" class="ui form">
      <div class="field required">
        <label>Contact ID</label>
        <input
          v-model="User.cid"
          type="number"
          name="contact-id"
          placeholder="Contact ID"
        />
      </div>
      <div class="field required">
        <label>First Name</label>
        <input
          v-model="User.firstname"
          type="text"
          name="first-name"
          placeholder="First Name"
        />
      </div>
      <div class="field required">
        <label>Last Name</label>
        <input
          v-model="User.lastname"
          type="text"
          name="last-name"
          placeholder="Last Name"
        />
      </div>
      <div class="field required">
        <label>Mobile No</label>
        <input
          v-model="User.mobile"
          type="text"
          name="mobile-no"
          placeholder="Mobile No"
        />
      </div>
      <div class="field">
        <label>Email</label>
        <input
          v-model="User.email"
          type="text"
          name="email"
          placeholder="Email"
        />
      </div>
      <div class="field">
        <label>Facebook</label>
        <input
          v-model="User.facebook"
          type="text"
          name="facebook"
          placeholder="Facebook"
        />
      </div>
      <div class="field">
        <label>Image Url</label>
        <input
          v-model="User.imageUrl"
          type="text"
          name="image-url"
          placeholder="Image Url"
        />
      </div>
      <div id="btn-group">
        <button class="ui primary button" type="submit">Save</button>
        <router-link to="/contacts">
          <button class="ui button">Discard</button>
        </router-link>
      </div>
    </form>
  </div>
  <div class="loader-active" v-if="isLoading">
    <div class="ui active inverted dimmer">
      <div class="ui text loader">Loading</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "EditContact",
  data() {
    return {
      isLoading: false,
      User: {
        cid: "",
        firstname: "",
        lastname: "",
        mobile: "",
        email: "",
        facebook: "",
        imageUrl: "",
      },
    };
  },

  mounted() {
    this.isLoading = true;
    const JWT_token = {
      headers: {
        Authorization: "Bearer " + this.$cookies.get("token"),
      },
    };
    const url =
      "https://as-backen-1474.onrender.com/contacts/" +
      this.$route.params.contactId;
    axios
      .get(url, JWT_token)
      .then((Response) => {
        console.log(Response.data);
        this.User = Response.data[0];
      })
      .catch((error) => {
        console.error();
      });
    setTimeout(() => {
      this.isLoading = false;
    }, 2000);
  },
  methods: {
    editContact() {
      this.isLoading = true;
      let pass = true;
      if (!this.User.cid) {
        alert("require cid !");
        pass = false;
      } else if (!this.User.firstname) {
        alert("require firstname !");
        pass = false;
      } else if (!this.User.lastname) {
        alert("require lasttname !");
        pass = false;
      } else if (!this.User.mobile) {
        alert("require mobile !");
        pass = false;
      } else if (!this.User.email) {
        alert("require email !");
        pass = false;
      } else if (!/^[0-9]+$/.test(this.User.mobile)) {
        alert("Mobile No. match only number !");
        pass = false;
      }

      if (pass) {
        const url =
          "https://as-backen-1474.onrender.com/contacts/" +
          this.$route.params.contactId;
        const JWT_token = {
          headers: {
            Authorization: "Bearer " + this.$cookies.get("token"),
          },
        };
        axios
          .put(url, this.User, JWT_token)
          .then((Response) => {
            alert("update success!");
            this.$router.push("/contacts");
          })
          .catch((error) => {
            console.log(error);
          });
      }
      setTimeout(() => {
        this.isLoading = false;
      }, 2000);
    },
  },
};
</script>
<style scoped>
#btn-group {
  margin-top: 2%;
  display: flex;
  justify-content: center;
  align-self: center;
}

#form-box {
  margin: 20px;
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
