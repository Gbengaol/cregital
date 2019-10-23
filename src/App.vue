<template>
  <div class="" id="app">
    <Description />
    <router-view />
  </div>
</template>
<script>
// @ is an alias to /src
import Description from "@/components/Description.vue";
export default {
  components: {
    Description
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 50px 100px;

  @media only screen and (max-width: 960px) {
    margin: 10px;
  }
}

#nav {
  padding: 30px;
  a {
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
