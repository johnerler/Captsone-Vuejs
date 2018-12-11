<template>
  <div class="home">
    <h1>Add New Expense</h1>
    
    Description: <input v-model="newExpenseDescription" type="text" /> 
    Amount:<input v-model="newExpenseAmount" type="decimal" /> 
    Date: <input v-model="newExpenseDate" type="date" /> 
    Category ID: <input v-model="newExpenseCategoryId" type="integer" /> 
    User Group ID: <input v-model="newExpenseUserGroupId" type="integer" />

    <button v-on:click="createExpense();" class="btn btn-primary">Create</button>
  
<!-- 
    <h1>{{ message }}</h1> -->
    <!-- <h2>{{ expense.total_expenses }}</h2> -->
    <!-- <div v-for="expense in expenses">
      <h2>{{ expense.description }} - {{ expense.amount }}</h2> -->
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
      message: "Shared Expenses",
      expenses: [],
      newExpenseDescription: "",
      newExpenseAmount: "",
      newExpenseDate: "",
      newExpenseCategoryId: "",
      newExpenseUserGroupId: ""
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/expenses").then(
      function(response) {
        console.log(response.data);
        this.expenses = response.data;
      }.bind(this)
    );
  },
  methods: {
    createExpense: function() {
      console.log("createExpense");
      var params = {
        description: this.newExpenseDescription,
        amount: this.newExpenseAmount,
        date: this.newExpenseDate,
        expense_category_id: this.newExpenseCategoryId,
        user_group_id: this.newExpenseUserGroupId
      };
      axios
        .post("http://localhost:3000/api/expenses", params)
        .then(
          function(response) {
            console.log(response);
            this.expenses.push(response.data);
            this.newExpenseDescription = "";
            this.newExpenseAmount = "";
            this.newExpenseDate = "";
            this.newExpenseCategoryId = "";
            this.newExpenseUserGroupId = "";
            this.$router.push("/about");
          }.bind(this)
        )
        .catch(
          function(error) {
            console.log(error.response);
          }.bind(this)
        );
    }
  },

  computed: {}
};
</script>
