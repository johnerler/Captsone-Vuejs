<template>
  <div class="home">
    <!-- <h1>{{ message }}</h1> -->
    <h1>Total Expenses: {{ user_group.total_expenses }}</h1>
    <h1>Shared Expenses: {{ user_group.shared_expenses }}</h1>
    <!-- <button @click="deleteAll(expense.id);">Reset</button> -->
    <h2>Search Expense</h2>
    <input type="text" v-model="searchFilter">


     <div class='container mb40 pt30'>
    <div class='row'>
     <div v-for="(expenses, category) in user_group.expenses_by_category">
      <h2 class="mt-0 mb10 text-uppercase">{{ category }}</h2>

      <div v-for="expense in filterBy(expenses, searchFilter, 'description')">
      <!-- <div v-for="expense in expenses"> -->
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

           <!-- <div class='container mb40 pt30'>
           <div class='row'>
               <div class="col-lg-4 mb40">
                    <div class="media">
                        <div class="d-flex mr-3">
                           <i class="icon-hover-1 bg-default ti-image icon-hover-default"></i>  
                        </div>
                        <div class="media-body">
                            <h4 class="mt-0 mb10 text-uppercase">Groceries</h4>

                          
                        </div>
                    </div>
                </div>
               <div class="col-lg-4 mb40">
                    <div class="media">
                        <div class="d-flex mr-3">
                           <i class="icon-hover-1 bg-default ti-briefcase icon-hover-default"></i>  
                        </div>
                        <div class="media-body">
                            <h4 class="mt-0 mb10 text-uppercase">Marketing</h4>
                           Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 mb40">
                    <div class="media">
                        <div class="d-flex mr-3">
                           <i class="icon-hover-1 bg-default ti-settings icon-hover-default"></i>  
                        </div>
                        <div class="media-body">
                            <h4 class="mt-0 mb10 text-uppercase">Development</h4>
                           Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
                        </div>
                    </div>
                </div>
           </div>
            <div class='row'>
               <div class="col-lg-4 mb40">
                    <div class="media">
                        <div class="d-flex mr-3">
                           <i class="icon-hover-1 bg-default ti-stats-up icon-hover-default"></i>  
                        </div>
                        <div class="media-body">
                            <h4 class="mt-0 mb10 text-uppercase">Analytics</h4>
                           Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
                        </div>
                    </div>
                </div>
               <div class="col-lg-4 mb40">
                    <div class="media">
                        <div class="d-flex mr-3">
                           <i class="icon-hover-1 bg-default ti-google icon-hover-default"></i>  
                        </div>
                        <div class="media-body">
                            <h4 class="mt-0 mb10 text-uppercase">Seo Optimize</h4>
                           Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 mb40">
                    <div class="media">
                        <div class="d-flex mr-3">
                           <i class="icon-hover-1 bg-default ti-trello icon-hover-default"></i>  
                        </div>
                        <div class="media-body">
                            <h4 class="mt-0 mb10 text-uppercase">Branding</h4>
                           Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
                        </div>
                    </div>
                </div>
           </div>
       </div> -->

<!--     <div v-for="(expenses, category) in user_group.expenses_by_category">
      <h2>{{ category }}</h2>

      <div v-for="expense in expenses">


        <p>
          {{ expense.description }} - {{ expense.amount }} - {{ expense.date }} -
          <button @click="deleteExpense(expense, expense.id);">Remove</button>
        </p>
      </div>
    </div>
    <div v-for="expense in user_group.expenses"> -->


      <!--
        <p>
          {{ expense.description }} - {{ expense.amount }} - {{ expense.date }} - {{ expense.expense_category }} -
          <button @click="deleteExpense(expense, expense.id);">Remove</button>
        </p>
      -->

      <p></p>
    </div>
  </div>
</template>

<style>
</style>

<script>
var axios = require("axios");
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Total Expenses",
      user_group: {},
      searchFilter: ""
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
