<template>
  <div id="app">
    <Header @titleInput="getTitle"/>
    <main>
      <Catalog :titleRequest="titleRequest"/>
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
  },
  data() {
    return {
      titleRequest: ""
    }
  },
  methods: {
    getTitle(input) {
      this.titleRequest = input;
    }
  }
}
</script>

<style lang="scss">

</style>
