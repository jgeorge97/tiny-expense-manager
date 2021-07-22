<template>
  <div class="card">
    <header class="card-header">
      <p class="card-header-title">
        {{ new Date(date).toLocaleString('en-IN') }}
      </p>
    </header>
    <div class="card-content">
      <div class="title"><span :class="isIncome?'has-text-success':'has-text-danger'">₹{{ amount }}</span></div>
      <div class="subtitle">{{ description }}</div>
    </div>
    <footer class="card-footer">
        <a @click="deleteItem" class="card-footer-item">Delete</a>
    </footer>
  </div>
</template>

<script>
export default {
  name: "Item",
  props: {
    description: String,
    amount: Number,
    isIncome: Boolean,
    date: String
  },
  methods: {
    deleteItem() {
      this.$emit('delete-item');
    }
  }
}
</script>

<style>

</style>