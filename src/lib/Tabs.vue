<template>
  <div class="wheel-tabs">
    <div class="wheel-tabs-nav" ref="container">
      <div
        class="wheel-tabs-nav-item"
        :class="{ selected: t === selected }"
        v-for="(t, index) in titles"
        :ref="
          (el) => {
            if (t === selected) selectedItem = el;
          }
        "
        @click="select(t)"
        :key="index"
      >
        {{ t }}
      </div>
      <div class="wheel-tabs-nav-indicator" ref="indicator"></div>
    </div>
    <div class="wheel-tabs-content">
      <component :is="current" :key="current.props.title" />
    </div>
  </div>
</template>
<script lang="ts">
import { computed, onMounted, ref, watchEffect } from "vue";
import Tab from "./Tab.vue";
export default {
  props: {
    selected: {
      type: String,
    },
  },
  setup(props, context) {
    const selectedItem = ref<HTMLDivElement>(null);
    const indicator = ref<HTMLDivElement>(null);
    const container = ref<HTMLDivElement>(null);

    onMounted(() => {
      watchEffect(
        () => {
          console.log("watchEffect执行了");
          const { width } = selectedItem.value.getBoundingClientRect();
          indicator.value.style.width = width + "px";
          const { left: left1 } = container.value.getBoundingClientRect();
          const { left: left2 } = selectedItem.value.getBoundingClientRect();
          const left = left2 - left1;
          indicator.value.style.left = left + "px";
        },
        { flush: "post" }
      );
    });

    const defaults = context.slots.default();
    const current = computed(() => {
      return defaults.filter((tag) => tag.props.title === props.selected)[0];
    });
    defaults.forEach((tag) => {
      //@ts-ignore
      if (tag.type.name !== Tab.name) {
        throw new Error("Tabs子标签必须是Tab");
      }
    });

    const titles = defaults.map((tag) => {
      return tag.props.title;
    });
    const select = (title: string) => {
      context.emit("update:selected", title);
    };
    return {
      defaults,
      titles,
      select,
      indicator,
      container,
      selectedItem,
      current,
    };
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
    position: relative;
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
    &-indicator {
      border: 1px solid blue;
      position: absolute;
      height: 3px;
      background: $blue;
      left: 0;
      width: 100px;
      bottom: -1px;
      transition: all 250ms;
    }
  }
  &-content {
    padding: 8px 0;
  }
}
</style>