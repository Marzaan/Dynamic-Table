<template>
  <div>
    <DynamicTable/>
  </div>
</template>

<script>
import DynamicTable from "./components/DynamicTable";

export default {
  name: "App",
  components: {
    DynamicTable,
  },
};
</script>

<!--
<style>
npm i --save-dev @types/vuelidate
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
-->