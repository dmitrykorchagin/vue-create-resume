<template>
  <div class="form-control">
    <label for="value">{{ label }}</label>
    <textarea
      id="value"
      rows="3"
      :value="modelValue"
      @input="change"
    ></textarea>
  </div>
</template>

<script>
export default {
  emits: ['update:modelValue'],
  props: {
    modelValue: String,
    label: String
  },
  methods: {
    change(e) {
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>
