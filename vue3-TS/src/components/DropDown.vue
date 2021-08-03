<template>
  <div class="dropdown" ref="dropDOwnRef">
    <a href="#" class="btn btn-outline-light my-2 dropdown-toggle" @click.prevent="dropOpen">{{title}}</a>
    <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1" :style="{display: 'block'}" v-show="isOpen">
      <li>
        <a class="dropdown-item" href="#">新建文章</a>
      </li>
      <li>
        <a class="dropdown-item" href="#">编辑资料</a>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted, watch } from 'vue'
import clickOut from '@/hooks/clickOut'

export default defineComponent({
  name: 'DropDown',
  props: {
    title: {
      type: String,
      required: true
    }
  },
  setup() {
    const isOpen = ref(false)
    const dropDOwnRef = ref<null | HTMLElement>(null)
    const dropOpen = () => {
      isOpen.value = !isOpen.value
    }
    const isClickOutside = clickOut(dropDOwnRef)
    watch(isClickOutside, () => {
      if (isOpen.value && isClickOutside.value) {
        isOpen.value = false
      }
    })
    // const handler = (e: MouseEvent) => {
    //   if (dropDOwnRef.value) {
    //     if (!dropDOwnRef.value.contains(e.target as HTMLElement) && isOpen.value === true) {
    //       isOpen.value = false
    //     }
    //   }
    // }
    // onMounted(() => {
    //   document.addEventListener('click', handler)
    // })
    // onUnmounted(() => {
    //   document.removeEventListener('click', handler)
    // })
    return {
      isOpen,
      dropOpen,
      dropDOwnRef
    }
  }
})
</script>

<style scoped>

</style>
