<template>
  <div class="home">
    <Editor msg="# Milkdown 💗 Vue2" />
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import Editor from "@/components/Editor";

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    Editor
  }
}
</script>
