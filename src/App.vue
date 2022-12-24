<template>
  <div id="app">
    <IpLookup />
  </div>
</template>

<script>
import IpLookup from "./components/Ip-Lookup.vue";
export default {
  name: "App",
  components: {
    IpLookup,
  },
};
</script>
<style>
#app {
  background: #e2e3e5;
  height:100%;
}
</style>