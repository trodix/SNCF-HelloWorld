<template>
  <div id="app">
    <Header/>
    <ListeGare/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import ListeGare from './components/ListeGare.vue'

export default {
  name: 'app',
  components: {
    Header,
    ListeGare
  }
}
</script>

<style>

.btn-primary {
  background-color: #009BD7;
}

</style>
