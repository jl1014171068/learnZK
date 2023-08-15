<template>
  <div class="App">
    <Card :result="list" />
  </div>
</template>

<script lang="js" setup name="App">
import Card from '@/components/Card.vue'
import list from '@/json/admin.js'
</script>
<style scoped lang="scss">
.App {
  text-align: left;
}
</style>
