<template>
  <div class="layout">
    <Topnav :toggleMenuButtonVisible="true" class="nav"/>
    <div class="content">
      <aside v-if="asideVisible">
        <h2>文档</h2>
        <ol>
          <li>
            <router-link to="/doc/intro">介绍</router-link>
          </li>
          <li>
            <router-link to="/doc/install">安装</router-link>
          </li>
          <li>
            <router-link to="/doc/get-start">开始使用</router-link>
          </li>
        </ol>
        <h2>组件列表</h2>
        <ol>
          <li>
            <router-link to="/doc/switch">Switch 开关</router-link>
          </li>
          <li>
            <router-link to="/doc/button">Button 按钮</router-link>
          </li>
          <li>
            <router-link to="/doc/dialog">Dialog 对话框</router-link>
          </li>
          <li>
            <router-link to="/doc/tabs">Tabs 标签页</router-link>
          </li>
          <li>
            <router-link to="/doc/radios">Radios 单选框</router-link>
          </li>
          <li>
            <router-link to="/doc/collapse">Collapse 折叠</router-link>
          </li>
        </ol>
      </aside>
      <main>
        <router-view/>
      </main>
    </div>
  </div>
</template>
<script lang="ts">
  import {inject, Ref} from 'vue';
  import Topnav from '../components/Topnav.vue';

  export default {
    components: {
      Topnav,
    },
    setup() {
      const asideVisible = inject<Ref<boolean>>('asideVisible'); //get
      return {asideVisible};
    },
  };
</script>
<style lang="scss" scoped>
  $aside-index: 10;
  .layout {
    display: flex;
    flex-direction: column;
    height: 100vh;

    > .nav {
      flex-shrink: 0;
      background: #b6dff9;
      box-shadow: 0 0 3px 1px rgba(0, 0, 0, 0.2);
    }

    > .content {
      flex-grow: 1;
      padding-top: 60px;
      padding-left: 156px;
      @media (max-width: 500px) {
        padding-left: 0;
      }
    }
  }

  .content {
    display: flex;

    > aside {
      flex-shrink: 0;
    }

    > main {
      flex-grow: 1;
      padding: 16px;
      background: #ffffff;
    }
  }

  aside {
    background: #b6dff9;
    box-shadow: 0 0 3px rgba(0, 0, 0, 0.2);
    width: 150px;
    padding: 16px 0;
    position: fixed;
    top: 0;
    left: 0;
    padding-top: 70px;
    height: 100%;
    z-index: $aside-index;

    > h2 {
      margin-bottom: 4px;
      padding: 0 16px;
    }

    > ol {
      > li {
        a {
          display: block;
          padding: 4px 16px;

          &:hover {
            background: #e6f7ff;
          }
        }

        > .router-link-active {
          background: white;
        }
      }
    }

    main {
      overflow: hidden;
    }
  }
</style>