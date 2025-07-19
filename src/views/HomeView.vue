<template>
  <MenuView />
  <div class="container">
    <ContactsView />
  </div>
</template>

<script>
// @ is an alias to /src
import ContactsView from '@/views/ContactsView.vue';
import MenuView from './MenuView.vue';

export default {
  name: 'HomeView',
  components: {
    ContactsView,
    MenuView
  }
}
</script>

<style scoped lang="scss">
  .container {
    padding: 10px;
  }
</style>
