<template>
  <b-navbar v-bind:fixed-top="true">
    <template slot="brand">
      <b-navbar-item tag="router-link" v-bind:to="{ path: '/' }">
        <img v-bind:src="image" alt="Lightweight UI components for Vue.js based on Bulma" />
      </b-navbar-item>
    </template>
    <template class="is-primary" slot="start">
      <router-link class="navbar-item" to="/">
        <b-icon icon="home" />Home
      </router-link>
      <router-link class="navbar-item" to="/about">
        <b-icon icon="help" />About
      </router-link>
    </template>

    <template slot="end">
      <b-navbar-item tag="div">
        <div class="buttons">
          <b-button
            class="button"
            icon-left="github"
            tag="a"
            href="https://github.com/alexisdavalos/dev_coding_challenge/tree/master/reviewapp"
            target="_blank"
          >
            <strong>Code Base</strong>
          </b-button>
        </div>
      </b-navbar-item>
    </template>
  </b-navbar>
</template>
<script>
import amazon_logo from "../assets/amazon_logo.png";
export default {
  data() {
    return {
      image: amazon_logo,
    };
  },
};
</script>
<style lang="scss" scoped>
@import "../styles/_navigation.scss";
</style>
