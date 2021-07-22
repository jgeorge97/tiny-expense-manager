<template>
  <div>
    <div class="columns is-centered">
      <div class="column is-half">
        <b-button type="is-primary is-light" expanded @click="isCardModalActive = true">New Income</b-button>
      </div>
    </div>
    <div class="columns is-centered">
      <div class="column is-half" v-if="incomeList.length !== 0">
        <Item v-for="income in incomeList" v-bind:key="income.id" :description="income.description" v-bind:amount="income.amount" :isIncome="true" :date="income.date" class="items" />
      </div>
      <div class="notification is-warning" v-else>
        No Incomes Found. Try Adding some.
      </div>
    </div>
    <b-modal v-model="isCardModalActive" :width="640" scroll="keep">
        <div class="card">
          <div class="card-content">
            <p class="title">New Income</p>
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
            <a href="#" class="card-footer-item" @click="addIncome">Save</a>
            <a href="#" class="card-footer-item" @click="isCardModalActive = false">Cancel</a>
          </footer>
        </div>
    </b-modal>
  </div>
</template>

<script>
import Item from "@/views/Item";
import { v4 as uuidv4 } from 'uuid';
export default {
  name: 'Incomes',
  data() {
    return {
      incomeList: [],
      isCardModalActive: false,
      description: '',
      amount: '',
    };
  },
  mounted() {
    this.getIncomes();
  },
  methods: {
    async getIncomes() {
      if(localStorage.getItem('income_items')) {
        const result = JSON.parse(localStorage.getItem('income_items'));
        result.sort((a, b) => new Date(b.date) - new Date(a.date));
        this.incomeList = result;
      }
    },
    async addIncome() {
      if(!this.description) {
        this.$buefy.notification.open({
          duration: 2500,
          message: `Description required!!`,
          type: 'is-danger',
          hasIcon: false
        });
        return;
      }

      if(!this.amount) {
        this.$buefy.notification.open({
          duration: 2500,
          message: `Amount required!!`,
          type: 'is-danger',
          hasIcon: false
        });
        return;
      }

      const newItem = {
        description: this.description,
        amount: this.amount,
        id: uuidv4(),
        date: new Date
      }

      this.incomeList = [...this.incomeList, newItem].sort((a, b) => new Date(b.date) - new Date(a.date));
      this.description = '';
      this.amount = '';
      this.isCardModalActive = false;
    }
  },
  components: {
    Item,
  },
  watch: {
    incomeList: {
      handler() {
        console.log("Storage updating");
        localStorage.setItem('income_items', JSON.stringify(this.incomeList));
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