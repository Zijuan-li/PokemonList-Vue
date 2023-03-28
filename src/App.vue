<template>
  <div id="app">
    <Input />
    <List pokePerPage="18" />
    <Pagination pokePerPage="18" />
  </div>
</template>

<script>
import Input from "./components/Input";
import List from "./components/List";
import Pagination from "./components/Pagination";

export default {
  name: "App",
  data() {
    return {
      currentPage: 1,
    };
  },
  components: {
    Input,
    List,
    Pagination,
  },
  methods: {
    onPageChange(page) {
      console.log(page);
      this.currentPage = page;
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
