<template>
  <div class="wrap-field">
    <h1
      class="edit"
      contenteditable="true"
      @focusout="$emit('updateValue', localValue)"
      @input="changeValue"
    >
      {{ value }}
    </h1>
    <div class="delete" @click="$emit('delete')">
      &#10060;
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      localValue: this.value
    }
  },
  emits: ['updateValue', 'delete'],
  props: {
    value: String
  },
  methods: {
    changeValue(e) {
      this.localValue = e.target.innerText
    }
  }
}
</script>
