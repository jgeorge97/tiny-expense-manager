<template>
  <div>
    <div class="columns is-centered">
      <div class="column is-half">
        <b-button type="is-primary is-light" expanded @click="isCardModalActive = true">New Expense</b-button>
      </div>
    </div>
    <div class="columns is-centered">
      <div class="column is-half" v-if="expenseList.length !== 0">
        <Item v-for="expense in expenseList" v-bind:key="expense.id" :description="expense.description" v-bind:amount="expense.amount" :date="expense.date" :isIncome="false" class="items" v-on:delete-item="deleteItem(expense.id)" />
      </div>
      <div class="notification is-warning" v-else>
        No Expenses Found. Try Adding some.
      </div>
    </div>
    <b-modal v-model="isCardModalActive" :width="640" scroll="keep">
        <div class="card">
          <div class="card-content">
            <p class="title">New Expense</p>
            <div class="content">
              <b-field label="Description">
                <b-input v-model="description"></b-input>
              </b-field>
              <b-field label="Amount">
                <b-input v-model.number="amount" type="number"></b-input>
              </b-field>
            </div>
          </div>
          <footer class="card-footer">
            <a href="#" class="card-footer-item" @click="addExpense">Save</a>
            <a href="#" class="card-footer-item" @click="isCardModalActive = false">Cancel</a>
          </footer>
        </div>
    </b-modal>
  </div>
</template>

<script>
import Item from "@/components/Item";
import { v4 as uuidv4 } from 'uuid';
export default {
  name: 'Expenses',
  data() {
    return {
      expenseList: [],
      isCardModalActive: false,
      description: '',
      amount: '',
    };
  },
  mounted() {
    this.getExpenses();
  },
  methods: {
    async getExpenses() {
      if(localStorage.getItem('expense_items')) {
        const result = JSON.parse(localStorage.getItem('expense_items'));
        result.sort((a, b) => new Date(b.date) - new Date(a.date));
        this.expenseList = result;
      }
    },
    async addExpense() {
      if(!this.description) {
        this.$buefy.snackbar.open({
          message: 'Description required!!',
          type: 'is-warning',
          position: 'is-bottom'
        });
        return;
      }

      if(!this.amount) {
        this.$buefy.snackbar.open({
          message: 'Amount required!!',
          type: 'is-warning',
          position: 'is-bottom'
        });
        return;
      }

      const newItem = {
        description: this.description,
        amount: this.amount,
        id: uuidv4(),
        date: new Date
      }

      this.expenseList = [...this.expenseList, newItem].sort((a, b) => new Date(b.date) - new Date(a.date));
      this.description = '';
      this.amount = '';
      this.isCardModalActive = false;
      this.$buefy.snackbar.open({
        message: 'Created!!',
        type: 'is-success',
        position: 'is-bottom'
      });
    },
    async deleteItem(id) {
      this.expenseList = this.expenseList.filter(function (ele) {
        return ele.id !== id
      });
      this.$buefy.snackbar.open({
        message: 'Deleted!!',
        type: 'is-success',
        position: 'is-bottom'
      });
    }
  },
  components: {
    Item,
  },
  watch: {
    expenseList: {
      handler() {
        console.log("Storage updating");
        localStorage.setItem('expense_items', JSON.stringify(this.expenseList));
      },
      deep: true,
    }
  }
}
</script>

<style>
.items {
  margin-bottom: 0.45rem;
}
</style>