<script setup>
import { RouterView } from 'vue-router'
import 'primeicons/primeicons.css';
import './assets/style.css'
</script>

<template>
  <div id="layout">
    <header>
      <div class="wrapper"></div>
      <nav>
        <RouterLink to="/"> <h1>🌍 Travel Journal 📓</h1></RouterLink>
      </nav>
    </header>
  </div>
  <RouterView />
</template>