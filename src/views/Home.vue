<template>
  <div class="home">

    <h1>Add New Expense</h1>
    
      <input type="text" v-model="url" class="form-control" placeholder="Enter receipt!"> 
      <img v-if="url" v-bind:src="url" alt="">
    <p></p>
    <button v-on:click="createReceipt();" class="btn btn-primary">Analyze Receipt </button>
    <!-- <div v-for="receipt in receipts"></div> -->
    <p></p>
   
    
    <input v-model="newExpenseDescription" type="text" placeholder="Description" />
    <p></p> 
    <input v-model="newExpenseAmount" type="decimal" placeholder="Amount"/> 
    <p></p>
    <input v-model="newExpenseDate" type="text" placeholder="Date" /> 
    <p></p>
    <p></p>
    <input v-model="newExpenseCategoryId" type="integer" list="categories" placeholder="Category"/> 
    <datalist id="categories">
      <option>1 - Rent</option>
      <option>2 - Utilities</option>
      <option>3 - Groceries</option>
      <option>4 - Leisure</option>
      <option>5 - Miscellaneous</option>
    </datalist>
    <p></p>
    <input v-model="newExpenseUserGroupId" type="integer" list="user_groups" placeholder="User Group" />
    <datalist id="user_groups">
      <option>1 - Erler Household</option>
      <option>2 - Smith Household</option>
    </datalist>
    <p></p>
    <p></p>
    <button v-on:click="createExpense();" class="btn btn-primary">Create Expense</button>
  
<!-- 
    <h1>{{ message }}</h1> -->
    <!-- <h2>{{ expense.total_expenses }}</h2> -->
    <!-- <div v-for="expense in expenses">
      <h2>{{ expense.description }} - {{ expense.amount }}</h2> -->
      <p></p>
    </div>
  </div>
</template>

<style>
</style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Shared Expenses",
      expenses: [],
      receipts: [],
      newExpenseDescription: "",
      newExpenseAmount: "",
      newExpenseDate: "",
      newExpenseCategoryId: "",
      newExpenseUserGroupId: "",
      url: "",
      totalAmount: "",
      filter: ""
    };
  },
  created: function() {
    // var params = {
    //   url: "https://qph.fs.quoracdn.net/main-qimg-8aa597ee2d773fce151545846c9d08a0"
    // };
    // axios.post("http://localhost:3000/api/receipts/analyze", params).then(
    //   function(response) {
    //     console.log(response.data);
    //   }.bind(this)
    // );
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
            this.$router.push("/expense");
          }.bind(this)
        )
        .catch(
          function(error) {
            console.log(error.response);
          }.bind(this)
        );
    },
    createReceipt: function() {
      console.log("createReceipt");
      console.log(this.url);
      var params = { url: this.url };
      axios
        .post("http://localhost:3000/api/receipts/analyze", params)
        .then(
          function(response) {
            var totalAmount = response.data.totalAmount.data;
            var merchantName = response.data.merchantName.data;
            var date = response.data.date.data;
            console.log(totalAmount, merchantName, date);
            this.newExpenseAmount = totalAmount;
            this.newExpenseDescription = merchantName;
            this.newExpenseDate = date;
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
