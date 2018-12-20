<template>
  <div class="home">
    
  
    <!-- <h1>Add New Expense </h1> -->
      <div class="page-titles title-dark pt30 pb20 mb70">
            <div class="container">
                <div class="row">
                    <div class="col-md-6">
                        <h4 ><span>Add New Expense</span></h4>   
                    </div>
                    <div class=" col-md-6 mb0">
                        <ol class="breadcrumb text-md-right">
                            
                        </ol>
                    </div>
                </div>
            </div>
        </div>
      <h3>Enter Reciept URL:</h3>
      <input type="text" v-model="url" class="form-control" placeholder="Enter receipt URL...!"> 
      <div>
        <img v-if="url" v-bind:src="url" alt="">
      </div>
      <p></p>
      <button v-on:click="analyzeReceiptUrl();" class="btn btn-outline-success mb5 btn-rounded">Analyze Receipt</button>
      <p></p>
      <h4>or</h4>
      <p></p>
      <h3>Upload Reciept:</h3>
      <input type="file" class="form-control" v-on:change="setFile($event)" ref="fileInput" >
      <div>
        <img v-if="fileUrl" v-bind:src="fileUrl" alt="">
      </div>
      <div>
        
      </div>

    <p></p>
    <!-- <div v-for="receipt in receipts"></div> -->
    <p></p>
    <h5>Description</h5>
    <input v-model="newExpenseDescription" type="input" class="form-control" placeholder="Enter description..." />
    <p></p> 
    <h5>Amount</h5>
    <input v-model="newExpenseAmount" type="input" class="form-control" placeholder="Enter amount..."/> 
    <p></p>
    <h5>Date</h5>
    <input v-model="newExpenseDate" type="input" class="form-control" placeholder="Enter date..." /> 
    <p></p>
    <h5>Category</h5>
    <input v-model="newExpenseCategoryId" type="integer" class="form-control" list="categories" placeholder="Enter category..."/> 
    <datalist id="categories">
      <option>1 - Rent</option>
      <option>2 - Utilities</option>
      <option>3 - Groceries</option>
      <option>4 - Leisure</option>
      <option>5 - Miscellaneous</option>
    </datalist>
    <p></p>
    <h5>User Group</h5>
    <input v-model="newExpenseUserGroupId" type="integer" class="form-control" list="user_groups" placeholder="Enter user group..." />
    <datalist id="user_groups">
      <option>1 - Erler Household</option>
      <option>2 - The Band</option>
    </datalist>
    <p></p>
    <p></p>
    <button v-on:click="createExpense();" class="btn btn-outline-success mb5 btn-rounded">Create Expense</button>
  
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
      receiptImage: "",
      url: "",
      fileUrl: "",
      totalAmount: ""
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
    setFile: function(event) {
      if (event.target.files.length > 0) {
        // get the file
        this.receiptImage = event.target.files[0];
        // set image
        var reader = new FileReader();
        reader.onload = function(e) {
          this.fileUrl = e.target.result;
        }.bind(this);
        reader.readAsDataURL(event.target.files[0]);
        // analyze receipt
        console.log("gonna analyzeReceiptBlob");
        var formData = new FormData();
        formData.append("image", this.receiptImage);
        axios.post("http://localhost:3000/api/receipts/analyze_file", formData).then(response => {
          console.log(response.data);
          // set the input variables for creating the expense
          var totalAmount = response.data.totalAmount.data;
          var merchantName = response.data.merchantName.data;
          var date = response.data.date.data;
          console.log(totalAmount, merchantName, date);
          this.newExpenseAmount = totalAmount;
          this.newExpenseDescription = merchantName;
          this.newExpenseDate = date;
        });
      }
    },
    createExpense: function() {
      console.log("createExpense");
      // var params = {
      //   description: this.newExpenseDescription,
      //   amount: this.newExpenseAmount,
      //   date: this.newExpenseDate,
      //   expense_category_id: this.newExpenseCategoryId,
      //   user_group_id: this.newExpenseUserGroupId
      var formData = new FormData();
      formData.append("description", this.newExpenseDescription);
      formData.append("amount", this.newExpenseAmount);
      formData.append("date", this.newExpenseDate);
      formData.append("expense_category_id", this.newExpenseCategoryId);
      formData.append("user_group_id", this.newExpenseUserGroupId);
      formData.append("date", this.newExpenseDate);

      axios
        .post("http://localhost:3000/api/expenses", formData)
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
    analyzeReceiptUrl: function() {
      console.log("analyzeReceiptUrl");
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
    // analyzeFile: function() {
    //   console.log("analyzeFile");
    //   console.log(this.receiptImage);
    //   var params = { receiptImage: this.receiptImage };
    //   axios
    //     .post("http://localhost:3000/api/receipts/analyze_file", params)
    //     .then(
    //       function(response) {
    //         var totalAmount = response.data.totalAmount.data;
    //         var merchantName = response.data.merchantName.data;
    //         var date = response.data.date.data;
    //         console.log(totalAmount, merchantName, date);
    //         this.newExpenseAmount = totalAmount;
    //         this.newExpenseDescription = merchantName;
    //         this.newExpenseDate = date;
    //       }.bind(this)
    //     )
    //     .catch(
    //       function(error) {
    //         console.log(error.response);
    //       }.bind(this)
    //     );
    // }
  },

  computed: {}
};
</script>
