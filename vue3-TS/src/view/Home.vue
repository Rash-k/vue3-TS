<template>
<div>1111111</div>
</template>

<script lang="ts">
import { useRoute } from 'vue-router'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Home',
  setup() {
    const route = useRoute()
    return {
      route
    }
  }
})
</script>

<style scoped>

</style>
