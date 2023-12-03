<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios';
import { ref } from 'vue';

axios.defaults.withCredentials = true;
axios.defaults.withXSRFToken = true;
const form = ref({
  email: null,
  password: null,
});

const user = ref();

async function onLogin() {
  await axios.get("http://localhost:8000/sanctum/csrf-cookie");
  await axios.post("http://localhost:8000/login", {
    email: form.value.email,
    password: form.value.password,
  });

 let {data} = await axios.get("http://localhost:8000/api/user");
  user.value = data;
}
</script>

<template>
  {{ user }}

  <form @submit.prevent="onLogin">
    <div>
      <input type="email" v-model="form.email">
    </div>
    <div>
      <input type="password" v-model="form.password">
    </div>
    <button>Se connecter</button>
  </form>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
