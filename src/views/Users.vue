<template>
  <div class="home">

   


        <div class="page-titles title-dark pt30 pb20 mb70">
            <div class="container">
                <div class="row">
                    <div class="col-md-6">
                        <h4 ><span>Users In Current Group</span></h4>
                    </div>
                    <div class=" col-md-6 mb0">
                        <ol class="breadcrumb text-md-right">
                            <li class="breadcrumb-item"><a href="index.html">Make Entry</a></li>
                        </ol>
                    </div>
                </div>
            </div>
        </div>
<!-- 
    <div class="title-heading1 mb40">
    <h1>Users In Current Group</h1>
  </div> -->

    
    <h2 v-for="user in user_group.users">
      <!-- <i class="icon-hover-1 bg-primary icon-profile-male icon-hover-default"></i>  -->
      <img v-bind:src="user.image_url" alt="">
     {{user.name}} - {{user.email}}</h2>

  </div>
</template>

<style>
</style>

<script>
var axios = require("axios");
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      user_group: {}
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/user_groups/").then(
      function(response) {
        console.log(response.data);
        this.user_group = response.data;
        this.expense = response.data;
      }.bind(this)
    );
  },
  methods: {},
  computed: {}
};
</script>
