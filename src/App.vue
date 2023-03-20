<template>
  <div id="app">
    <header class="header">
      <h1 class="header__heading">send a request</h1>
    </header>
    
    <FormMain />
    <footer class="footer">
      <div class="footer__group">
        <p class="footertext highlight">© 2022 LUXURY RESIDENCE</p>
        <a href="#" class="footertext">privacy policy</a>
      </div>
    </footer>
  </div>
</template>

<script>
import FormMain from './components/FormMain.vue'

export default {
  name: 'App',
  components: {
    FormMain
  }
}
</script>

