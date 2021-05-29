<template>
  <div id="app">
    <add-request/>
    <request-list/>
  </div>
</template>

<script>

import RequestList from "./components/RequestList.vue";
import AddRequest from "./components/AddRequest.vue";

export default {
  name: "app",
  components: {
    RequestList,
    AddRequest
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}
</style>
