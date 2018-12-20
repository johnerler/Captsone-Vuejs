<template>
  <div class="login">
    <div class="container">
      <form v-on:submit.prevent="submit();">
        <h1>Login</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group"><label><h5>Email:</h5></label> <input type="email" class="form-control" v-model="email" placeholder="Enter email..." /></div>
        <div class="form-group">
          <label><h5>Password:</h5></label> <input type="password" class="form-control" v-model="password" placeholder="Enter password..." />
        </div>
        <input type="submit" class="btn btn-success mb5 btn-rounded" value="Submit" />
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  template: "#login-page",
  data: function() {
    return {
      email: "",
      password: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        email: this.email,
        password: this.password
      };
      axios
        .post("http://localhost:3000/api/sessions", params)
        .then(response => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          this.$router.push("/Users");
        })
        .catch(error => {
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    }
  }
};
</script>
