<template>
  <div id="app">
    <div class="pagesLayout">
      <Auth/>
    </div>
  </div>
</template>

<script>

import Auth from "./pages/Auth";

export default {
  name: 'App',
  components: {
    Auth,
  },
}
</script>

<style lang="scss">
@import 'assets/styles/main';

.pagesLayout {
  display: grid;
  width: 100%;
  grid-template-columns: 0.1fr 1fr;
}

.pagesLayout_contentRight {
  padding: 0.25vw 3vw;
}
</style>
