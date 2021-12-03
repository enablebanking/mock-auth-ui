<template>
  <div class="home">
    <h1 class="text-center m-4">Mock Authentication</h1>
    <Auth/>
    <div id="nav">
      <a href="https://github.com/enablebanking/mock-auth-ui#readme" target="_blank">
        About this project on GitHub
      </a>
    </div>
  </div>
</template>

<script>
import Auth from '@/components/Auth.vue'

export default {
  name: 'Home',
  components: {
    Auth
  }
}
</script>

<style lang="scss">
.home {
  img {
    width: 30%;
    max-width: 300px;
    margin: 1em auto 1em auto;
    display: block;
  }
}
#nav {
  padding: 30px;
  text-align: center;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
