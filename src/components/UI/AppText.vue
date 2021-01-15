<template>
  <div class="wrap-field">
    <p
      class="edit"
      contenteditable="true"
      @focusout="$emit('updateValue', localValue)"
      @input="changeValue"
    >
      {{ value }}
    </p>
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
