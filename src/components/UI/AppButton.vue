<template>
  <button
    class="btn"
    :disabled="disabled"
    :class="color"
    @click="$emit('click')"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {
  emits: ['click'],
  props: {
    color: {
      type: String,
      default: '',
      validator(value) {
        return ['primary', 'warning', 'danger'].indexOf(value) !== -1
      }
    },
    disabled: {
      type: Boolean,
      default: false
    }
  }
}
</script>
