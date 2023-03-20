<template>
  <main class="main">
    <section class="main__info">
      
      <p class="main__description">Leave your contacts and we will contact you as soon as possible</p>
      <img :src="icon" alt="" class="main__logo">
    </section>
    <FormForm/>
  </main>
</template>

<script>
import FormForm from '@/components/FormForm.vue'
export default {
  name: 'FormMain',
  data() {
    return {
      icon: require('../assets/images/icon.png')
    }
  },
  components: {
    FormForm
  }
}
</script>