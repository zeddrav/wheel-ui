<template>
  <button
    class="wheel-switch"
    :class="{ 'wheel-checked': value }"
    @click="toggle"
  >
    <span></span>
  </button>
</template>
<script lang="ts">
export default {
  props: {
    value: Boolean,
  },
  setup(props, context) {
    const toggle = () => {
      context.emit("update:value", !props.value);
    };
    return { toggle };
  },
};
</script>
<style lang="scss">
$h: 22px;
$h2: $h - 4px;
.wheel-switch {
  height: $h;
  width: $h * 2;
  border: none;
  background: #bfbfbf;
  border-radius: $h/2;
  position: relative;
  transition: 0.5s;
  > span {
    position: absolute;
    top: 2px;
    left: 2px;
    height: $h2;
    width: $h2;
    background: #ffffff;
    border-radius: $h2/2;
    transition: 0.5s;
  }
  &.wheel-checked {
    background: #1890ff;
    > span {
      left: calc(100% - #{$h2} - 2px);
    }
  }
  &:focus {
    outline: none;
  }
  &:active {
    > span {
      width: $h2 + 4px;
    }
  }
  &.wheel-checked:active {
    > span {
      width: $h2 + 4px;
      margin-left: -4px;
    }
  }
  &[disabled] {
    cursor: not-allowed;
    &:active {
      > span {
        width: $h2;
      }
    }
  }
}
</style>