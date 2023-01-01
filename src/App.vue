<script setup>
import WordPairs from "@/components/WordPairs.vue";
</script>

<template>

  <body>
  <header class="nav-logo">
    <nav><img class="logo" src="https://weborigo.com/assets/img/logos/weborigo-logo-orange-black.svg" alt="weborigo"></nav>
  </header>

  <main class="container">
    <WordPairs/>
  </main>
  </body>

</template>

<style>

.nav-logo {
  display: flex;
  justify-content: center;
  align-items: center;

}
.logo {
  margin: 36px;
  width: 200px;
  height: 40px;

}
body {
  background-color: orangered;
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}


</style>
