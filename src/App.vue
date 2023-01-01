<script setup>
import QrCard from './components/QrCard.vue'
</script>

<template>
  <div>
    <a href="#">
      <img src="/logo.svg" class="logo" alt="Qrcode logo" />
    </a>
  </div>
  <QrCard class="mt-md" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: all 0.3s ease;
}
.logo:hover {
  filter: drop-shadow(0 0 2em var(--color-positive));
}
</style>
