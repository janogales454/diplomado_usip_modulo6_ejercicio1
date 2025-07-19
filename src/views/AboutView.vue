<template>
  <MenuView />
  <div class="container">
    <div class="about">
      <h1>This is an about page</h1>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import MenuView from './MenuView.vue';

export default {
  name: 'AboutView',
  components: {
    MenuView,
  }
}
</script>

<style scoped lang="scss">
  .container {
    padding: 10px;
  }
</style>
