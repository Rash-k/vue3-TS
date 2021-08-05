<template>
<div>3213122312312312312</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { useRoute } from 'vue-router'

export default defineComponent({
  name: 'login',
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
