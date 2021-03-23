<template>
  <div class="wheel-tabs">
    <div class="wheel-tabs-nav">
      <div
        class="wheel-tabs-nav-item"
        :class="{ selected: t === selected }"
        v-for="(t, index) in titles"
        @click="select(t)"
        :key="index"
      >
        {{ t }}
      </div>
    </div>
    <div class="wheel-tabs-content">
      <component
        class="wheel-tabs-content-item"
        :class="{ selected: c.props.title === selected }"
        v-for="c in defaults"
        :is="c"
      />
    </div>
  </div>
</template>
<script lang="ts">
import { computed } from "vue";
import Tab from "./Tab.vue";
export default {
  props: {
    selected: {
      type: String,
    },
  },
  setup(props, context) {
    const defaults = context.slots.default();
    defaults.forEach((tag) => {
      if (tag.type !== Tab) {
        throw new Error("Tabs子标签必须是Tab");
      }
    });
    const current = computed(() => {
      console.log("重新return");
      return defaults.filter((tag) => {
        return tag.props.title === props.selected;
      })[0];
    });

    const titles = defaults.map((tag) => {
      return tag.props.title;
    });
    const select = (title: string) => {
      context.emit("update:selected", title);
    };
    return { defaults, titles, current, select };
  },
};
</script>
<style lang="scss">
$blue: #40a9ff;
$color: #333;
$border-color: #d9d9d9;
.wheel-tabs {
  &-nav {
    display: flex;
    color: $color;
    border-bottom: 1px solid $border-color;
    &-item {
      padding: 8px 0;
      margin: 0 16px;
      cursor: pointer;
      &:first-child {
        margin-left: 0;
      }
      &.selected {
        color: $blue;
      }
    }
  }
  &-content {
    padding: 8px 0;
    &-item {
      display: none;
      &.selected {
        display: block;
      }
    }
  }
}
</style>