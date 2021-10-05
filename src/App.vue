<template>
  <div id="app">
    <Header/>
    <main>
      <Catalog/>
    </main>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Catalog from './components/Catalog.vue';

export default {
  name: 'App',
  components: {
    Header,
    Catalog
  }
}
</script>

<style lang="scss">

</style>
