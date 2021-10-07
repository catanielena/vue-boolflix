<template>
  <div id="app">
    <Header @titleInput="getTitle"/>
    <main>
      <Catalog :titleRequest="titleRequest" @collection="getCatalog"/>
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
      titleRequest: "",
      data: null
    }
  },
  methods: {
    getTitle(input) {
      this.titleRequest = input;
    },
    getCatalog(e) {
      this.data = e
    }
  }
}
</script>

<style lang="scss">
@import './assets/style/common';

#app {
  background-color: $mainColor;
  min-height: 100vh; 
}

main {
  margin-top: 2.75rem;
  overflow-y: hidden;
}
</style>
