/* eslint-disable */
<template>

  <div class="container">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active"></li>
        </ul>
        <b-button id="show-btn" @click="$bvModal.show('bv-modal-example')">Log In</b-button>

        <b-modal ref="login-modal" id="bv-modal-example" hide-footer>
          <table class="table">
            <tbody>
            <tr>
              <td>Email:</td>
              <td>
                <input v-model="email" required>
              </td>
            </tr>
            <tr>
              <td>Password:</td>
              <input v-model="password" type="password" required>
            </tr>
            <tr>
              <td>
              </td>
              <td>
                <button @click="logIn">Log In!</button>&nbsp;&nbsp;
                <button @click="logOut">Log Out!</button>
              </td>
            </tr>
            </tbody>
          </table>
        </b-modal>
      </div>
    </nav>
    0
  </div>
</template>
<script>
// eslint-disable-next-line no-unused-vars
import axios from 'axios';

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  props: {
    msg: String
  },
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    logIn() {
      const self = this;
      axios.post(this.$API_URL + 'registration/prism-portal-auth', {
        email: this.email,
        password: this.password,
      }).then(function (res) {
        const dat = res.data;
        const parsed = JSON.stringify(dat);
        localStorage.setItem('user_info', parsed);
        self.$API_KEY = dat.api_key;
        console.log("user info saved into local storage");
        self.$refs['login-modal'].hide();
      }).catch(function (error) {
        console.log("error")
        console.log(error);
        self.$refs['login-modal'].hide()
      });
    },
    logOut() {
      //user_info is removed from local storage
      //corresponding variables are reset to blank
      const self = this;
      if (localStorage.getItem('user_info')) {
        self.password = '';
        self.email = '';
        self.$API_KEY = '';
        localStorage.removeItem('user_info');
      }
      this.$refs['login-modal'].hide()
    }
  },
  created() {
    if (localStorage.getItem('user_info')) {
      try {
        const user_info = localStorage.getItem('user_info');
        const user_info_parsed = JSON.parse(user_info);
        this.email = user_info_parsed.email;
        this.$API_KEY = user_info_parsed.api_key;
      } catch (e) {
        localStorage.removeItem('user_info');
      }
      console.log("API_KEY", this.$API_KEY)
    } else {
      console.log("NO API_KEY", this.$API_KEY)
    }
  }
}
</script>

<style>
h3 {
  margin-bottom: 5%;
}
</style>
