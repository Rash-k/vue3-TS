<template>
  <div class="validate-input-container pb-3">
<!--    <input type="text"-->
<!--    class="form-control"-->
<!--    :class="{'is-invalid': inputRef.error}"-->
<!--    v-model="inputRef.val"-->
<!--    @blur="validateInput"-->
<!--    >-->
    <label class="form-label">邮箱地址</label>
    <input type="text"
           class="form-control"
           :class="{'is-invalid': inputRef.error}"
           :value="inputRef.val"
            @blur="validateInput"
           @input="updateValue"
           v-bind="$attrs"
    >
    <label class="form-label">密码</label>
    <input type="text"
           class="form-control"
           :class="{'is-invalid': inputRef.error}"
           :value="inputRef.val"
           @blur="validateInput"
           @input="updateValue"
           v-bind="$attrs"
    >
    <span v-if="inputRef.error" class="invalid-feedback">{{inputRef.message}}</span>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, PropType, onMounted } from 'vue'
import { emitter } from './ValidateForm.vue'
const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
interface RuleProp {
  type: 'required' | 'email' | 'user';
  message: string
}

export type RulesProp = RuleProp[]

export default defineComponent({
  name: 'ValidateInput',
  props: {
    rules: Array as PropType<RulesProp>,
    user: Array as PropType<RulesProp>,
    modelValue: String
  },
  inheritAttrs: false,
  setup(props, context) {
    const inputRef = reactive({
      val: props.modelValue || '',
      error: false,
      message: ''
    })
    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value
      inputRef.val = targetValue
      context.emit('update:modelValue', targetValue)
    }
    const validateInput = () => {
      if (props.rules) {
        const allPassed = props.rules.every(rule => {
          let passed = true
          // console.log(rule)
          inputRef.message = rule.message
          // console.log(rule.message)
          switch (rule.type) {
            case 'required':
              passed = (inputRef.val.trim() !== '')
              break
            case 'email':
              passed = (emailReg.test(inputRef.val))
              break
            default:
              break
          }
          return passed
        })
        inputRef.error = !allPassed
        return allPassed
      }
      return true
    }
    onMounted(() => {
      emitter.emit('form-item-created', validateInput)
    })
    return {
      inputRef,
      validateInput,
      updateValue
    }
  }
})
</script>

<style scoped>

</style>
