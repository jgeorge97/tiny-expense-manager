<template>
  <div class="home">
    <b-tabs position="is-centered" expanded type="is-toggle" class="">
      <b-tab-item label="Expense">
        <Expenses />
      </b-tab-item>
      <b-tab-item label="Income">
        <Incomes />
      </b-tab-item>
    </b-tabs>
  </div>
</template>

<script>
// @ is an alias to /src
import Expenses from "@/components/Expenses";
import Incomes from "@/components/Incomes";
export default {
  name: 'Home',
  components: {
    Expenses,
    Incomes,
  }
}
</script>
<style>
  .tabs {
    margin-top: 0.5rem;
    margin-left: 0.5rem;
    margin-right: 0.5rem;
  }
</style>
