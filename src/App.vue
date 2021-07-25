<template>
  <div id="app">
    <b-navbar type="is-primary">
      <template #brand>
        <b-navbar-item tag="router-link" :to="{ path: '/' }">
          <b>Tiny Expense Manager</b> 
        </b-navbar-item>
      </template>
      <template #start>
      </template>
      <template #end>
        <b-navbar-item tag="router-link" :to="{ path: '/' }">
          Home
        </b-navbar-item>
        <b-navbar-item tag="router-link" :to="{ path: '/about' }"> About </b-navbar-item>
        <b-navbar-item tag="router-link" :to="{ path: '/privacy-policy' }"> Privacy Policy </b-navbar-item>
      </template>
    </b-navbar>
    <router-view/>
    <footer class="footer">
      <div class="content has-text-centered">
        <p>
          <strong>Tiny Expense Manager</strong> by <a href="https://github.com/jgeorge97">George Martin Jose</a>
        </p>
      </div>
    </footer>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  }
}
</script>

<style>
</style>
