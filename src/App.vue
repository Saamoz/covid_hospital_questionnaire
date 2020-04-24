<template>
  <div id="app">
    <nav class="navbar navbar-default">
      <div class="container-fluid">
        <ul class="nav navbar-nav">
          <li>
            <router-link to="/">Home</router-link>
          </li>
        </ul>
      </div>
    </nav>
    <router-view class="view"></router-view>
  </div>
</template>

<script>
import Vue from "vue";
import VueRouter from "vue-router";

Vue.use(VueRouter)

const Home = () => import("./views/Home.vue");

const router = new VueRouter({
  mode: "history",
  base: __dirname,
  routes: [
    { path: "/", component: Home},
    { path: "/test", component: Home}
  ]
});

export default {
  name: 'App',
  router: router
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
