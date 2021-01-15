<template>
  <div class="form-control">
    <label for="type">Тип блока</label>
    <select id="type" :value="modelValue" @input="change">
      <option value="title">Заголовок</option>
      <option value="subtitle">Подзаголовок</option>
      <option value="avatar">Аватар</option>
      <option value="text">Текст</option>
    </select>
  </div>
</template>

<script>
export default {
  emits: ['update:modelValue'],
  props: {
    modelValue: {
      type: String,
      default: 'title'
    }
  },
  methods: {
    change(e) {
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>

<style scoped></style>
