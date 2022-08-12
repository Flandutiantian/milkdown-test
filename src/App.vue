<template>
  <div id="app">
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav> -->
    <router-view/>
  </div>
</template>

<style type="test/css">
.paragraph{
  text-align: left;
} 
div > p > .image-container {
    display: flex;
    text-align: center;
    font-size: 0;
    max-width: 70%;
    justify-content: center;
    margin-left: auto; 
    margin-right:auto; 
    display:block;
}

.milkdown .editor .tableWrapper {
    overflow-x: auto;
    margin: 0 auto;
    max-width: 70%;
    text-align: center;      
}

.milkdown .tableWrapper table {
    width: calc(100% - 2em)!important;
    margin: 1em 0 1em 1em!important;
   
    /* max-width: 100%; */
   
}

.milkdown .editor>* {
    margin: 0;
}
.milkdown .editor p {
    font-size: 16px;
    line-height: 1.5;
    letter-spacing: 0.5px;
    /* text-align: left; */
}
</style>

<style lang="scss">

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
