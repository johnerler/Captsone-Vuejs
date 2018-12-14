<template>
  <div class="home">
    <!-- <h1>{{ message }}</h1> -->
    <h1>Total Expenses: {{ user_group.total_expenses }}</h1>
    <h1>Shared Expenses: {{ user_group.shared_expenses }}</h1>
    <!-- <button @click="deleteAll(expense.id);">Reset</button> -->
    

     <div class='container mb40 pt30'>
    <div class='row'>
     <div v-for="(expenses, category) in user_group.expenses_by_category">
      <h2 class="mt-0 mb10 text-uppercase">{{ category }}</h2>

      <div v-for="expense in expenses">
        <div class="col-lg-12 mb40">

        <p><i class="icon-hover-1 bg-default ti-settings icon-hover-default"></i>
          {{ expense.description }} - {{ expense.amount }} - {{ expense.date }} -
          <button @click="deleteExpense(expense, expense.id);">Remove</button>
        </p>
      </div>
      </div>
    </div>
</div>
</div>

 

      <p></p>
    </div>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Total Expenses",
      user_group: {}
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/user_groups").then(
      function(response) {
        console.log(response.data);
        this.user_group = response.data;
        this.expense = response.data;
      }.bind(this)
    );
  },
  methods: {
    deleteExpense: function(index, id) {
      console.log(id);
      axios.delete("http://localhost:3000/api/expenses/" + id, {
        // request: 4,
        // id: id
      });
      this.$router.push("/");
      // .then(
      //   function(response) {
      //     //   // Remove index from users
      //     // this.expense.splice(index, 1);
      //     // alert(response.data);
      //   }.bind(this)
      // )
      // .catch(function(error) {
      //   console.log(error);
      // });
    }
  },
  deleteAll: function(expense) {
    console.log(expense);
    axios.delete("http://localhost:3000/api/expenses/");
  },
  computed: {}
};
</script>