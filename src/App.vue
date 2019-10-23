<template>
  <div class="" id="app">
    <router-view />
  </div>
</template>

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
