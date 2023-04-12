<template>
  <!--   <h1 class="ui center aligned header">Add Contact</h1>
  <div class = "ui container">
    <div class="ui medium header">Cid<span  id = "st-text">*</span></div>
    
      <div class="ui fluid icon input">
        <input type="number" placeholder="cid" v-model="User.cid">
      </div>
    
    <div class="ui medium header">Firstname <span  id = "st-text">*</span></div>
    
      <div class="ui fluid icon input">
        <input type="text" placeholder="Firstname" v-model="User.firstname">
      </div>
   
    <div class="ui medium header">Lastname<span  id = "st-text">*</span></div>
    
      <div class="ui fluid icon input">
        <input type="text" placeholder="Firstname" v-model="User.lastname">
      </div>
  
    <div class="ui medium header">Mobile No.<span  id = "st-text">*</span></div>
    
      <div class="ui fluid icon input">
        <input type="text" placeholder="mobile" v-model="User.mobile">
      </div>
   
    <div class="ui medium header">Facebook</div>
   
      <div class="ui fluid icon input">
        <input type="text" placeholder="facebook" v-model="User.facebook">
      </div>
  
    <div class="ui medium header">Imageurl</div>
    
      <div class="ui fluid icon input">
        <input type="text" placeholder="imageUrl" v-model="User.imageUrl">
      </div>

        <div id="btn-group">
          <button class="ui primary button"  @click="addContact">
          Save
        </button>
        <router-link to= "/contacts">
          <button class="ui button">Discard</button>
        </router-link>
        
        </div>
       </div> -->

  <div id="form-box" class="ui segment">
    <h2 class="ui header">
      Contact
      <a class="ui teal label">Add</a>
    </h2>
    <div class="ui divider"></div>
    <form class="ui form">
      <div class="field required">
        <label>Contact ID</label>
        <input
          v-model="formInput.cid"
          type="number"
          name="contact-id"
          placeholder="Contact ID"
        />
      </div>
      <div class="field required">
        <label>First Name</label>
        <input
          v-model="formInput.firstname"
          type="text"
          name="first-name"
          placeholder="First Name"
        />
      </div>
      <div class="field required">
        <label>Last Name</label>
        <input
          v-model="formInput.lastname"
          type="text"
          name="last-name"
          placeholder="Last Name"
        />
      </div>
      <div class="field required">
        <label>Mobile No</label>
        <input
          v-model="formInput.mobile"
          type="text"
          name="mobile-no"
          placeholder="Mobile No"
        />
      </div>
      <div class="field">
        <label>Email</label>
        <input
          v-model="formInput.email"
          type="text"
          name="email"
          placeholder="Email"
        />
      </div>
      <div class="field">
        <label>Facebook</label>
        <input
          v-model="formInput.facebook"
          type="text"
          name="facebook"
          placeholder="Facebook"
        />
      </div>
      <div class="field">
        <label>Image Url</label>
        <input
          v-model="formInput.imageUrl"
          type="text"
          name="image-url"
          placeholder="Image Url"
        />
      </div>
      <div class="ui buttons">
        <button @click="getForm" class="ui primary button">Save</button>
        <button class="ui button">Discard</button>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AddContact",
  data() {
    return {
      formInput: {
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
  methods: {
    addContact() {
      const JWT_token = {
        headers: {
          Authorization: "Bearer " + this.$cookies.get("token"),
        },
      };
      const url = "https://as-backen-1474.onrender.com/contacts/";
      axios
        .post(url, this.User, JWT_token)
        .then((Response) => {
          alert("Add new User success!");
          this.$router.push("/contacts");
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
<style></style>
