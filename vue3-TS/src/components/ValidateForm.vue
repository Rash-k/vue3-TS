<template>
  <form class="validate-form-container">
    <slot name="default"></slot>
    <div class="submit-area" @click.prevent="submitForm">
      <slot name="submit">
        <button type="submit" class="btn btn-primary">提交</button>
      </slot>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, onUnmounted } from 'vue'
import mitt from 'mitt'
type ValidateFun = () => boolean
export const emitter = mitt()
export default defineComponent({
  name: 'ValidateForm',
  emits: ['form-submit'],
  setup(props, context) {
    let funArr: ValidateFun[] = []
    const submitForm = () => {
      const result = funArr.map(func => func()).every(result => result)
      context.emit('form-submit', result)
    }
    const callback = (func: ValidateFun | any) => {
      funArr.push(func)
    }
    emitter.on('form-item-created', callback)
    onUnmounted(() => {
      emitter.off('form-item-created', callback)
      funArr = []
    })
    return {
      submitForm
    }
  }
})
</script>

<style scoped>

</style>
