<template>
  <div class="home">
    <!--<img alt="Vue logo" src="../assets/logo.png">-->
    <Colorizer />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Colorizer from '@/components/Colorizer.vue'; // @ is an alias to /src

@Component({
  components: {
    Colorizer,
  },
})
export default class Home extends Vue {}
</script>
