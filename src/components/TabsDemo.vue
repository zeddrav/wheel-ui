<template>
  <br />
  <h2>Tabs 标签页</h2>
  <br />
  <Demo :component="Tabs1Demo" />
</template>
<script lang="ts">
import Demo from "./Demo.vue";
import Tabs1Demo from "../apply/tabs/Tabs1.demo.vue";
export default {
  components: {
    Demo,
  },
  setup() {
    return { Tabs1Demo };
  },
};
</script>