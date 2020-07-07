<template>
  <div class="button-card">
    <div>
      <slot name="title"></slot>
    </div>
    <button :class="[`${type}`, `${size}`]" @click="handleClick">
      <slot name="button" v-bind:children="contentObj">{{ contentObj.text }}</slot>
    </button>
    <p>
      <slot>help</slot>
    </p>
  </div>
</template>

<script>
export default {
  name: 'BaseButton',
  props: {
    type: {
      type: String,
      default: ''
    },
    size: {
      type: String,
      default: ''
    },
    contentObj: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  methods: {
    handleClick() {
      this.$emit('click')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.button-card {
  background: #eee;
  border-radius: 8px;
  padding: 24px;
}
button {
  border-radius: 4px;
}
.success {
  background-color: rgb(149, 204, 149);
  border: 1px solid rgb(149, 204, 149);
  color: #fff;
}
.warning {
  background-color: orange;
  border: 1px solid orange;
  color: #fff;
}
.danger {
  background-color: red;
  border: 1px solid red;
  color: #fff;
}
.small {
  height: 20px;
  padding: 0 8px;
}
.middle {
  height: 32px;
  padding: 0 12px;
}
.large {
  height: 40px;
  padding: 0 20px;
}
</style>

