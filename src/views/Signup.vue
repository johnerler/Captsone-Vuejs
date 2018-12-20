<template>
  <div class="signup">
    <div class="container">
      <form v-on:submit.prevent="submit();">
        <h1>Signup</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group"><label>Name:</label> <input type="text" class="form-control" v-model="name" /></div>
        <div class="form-group"><label>Email:</label> <input type="email" class="form-control" v-model="email" /></div>
        <div class="form-group">
          <label>User Group ID:</label> <input type="integer" class="form-control" v-model="user_group_id" />
        </div>
        <div class="form-group">
          <label>Password:</label> <input type="password" class="form-control" v-model="password" />
        </div>
        <div class="form-group">
          <label>Password confirmation:</label>
          <input type="password" class="form-control" v-model="passwordConfirmation" />
        </div>
        <input type="submit" class="btn btn-success mb5 btn-rounded" value="Submit" />
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      name: "",
      email: "",
      password: "",
      user_group_id: "",
      passwordConfirmation: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        name: this.name,
        email: this.email,
        user_group_id: this.user_group_id,
        password: this.password,
        password_confirmation: this.passwordConfirmation
      };
      axios
        .post("http://localhost:3000/api/users", params)
        .then(response => {
          this.$router.push("/login");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
