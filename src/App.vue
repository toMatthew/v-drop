<template>
<div>
  <vdrop v-model="money" :min="min" :max="max" :speed="speed"></vdrop>
  {{money}}
</div>
</template>

<script>
import vdrop from "./vdrop";
export default {
  components : {
    vdrop
  },
  name: 'app',
  data () {
    return {
      money: 200,
      min : 100,
      max : 3000,
      speed : 10
    }
  }
}
</script>

<style>

</style>
