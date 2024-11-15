<script >
import WeddingCountdownComponent from "./components/wedding-countdown.component.vue";
import WeddingCountdown from "./components/wedding-countdown.component.vue";

export default {
  name: "app",
  components: {WeddingCountdown, WeddingCountdownComponent},


  title: 'Boda',

  created() {
    console.log('App created');
  }
}
</script>

<template>
  <wedding-countdown></wedding-countdown>
  <router-view></router-view>
</template>

<style >

</style>
