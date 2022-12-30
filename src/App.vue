<template>
  <a-layout style="min-height: 100vh; min-width: 100vw">
    <a-layout-sider v-model:collapsed="collapsed" collapsible>
      <div class="logo"></div>
      <a-menu v-model:selectedKeys="selectedKeys" theme="dark" mode="inline">
        <router-link class="link" to="/">
          <a-menu-item key="1">
            <home-outlined />
            <span>Home</span>
          </a-menu-item>
        </router-link>
        <router-link class="link" to="/option">
          <a-menu-item key="2">
            <desktop-outlined />
            <span>Option 2</span>
          </a-menu-item>
        </router-link>
        <router-link class="link" to="/management-user">
          <a-menu-item key="3">
            <desktop-outlined />
            <span>Management user</span>
          </a-menu-item>
        </router-link>
        <a-sub-menu key="sub1">
          <template #title>
            <span>
              <user-outlined />
              <span>User</span>
            </span>
          </template>
          <a-menu-item key="4">Tom</a-menu-item>
          <a-menu-item key="5">Bill</a-menu-item>
          <a-menu-item key="6">Alex</a-menu-item>
        </a-sub-menu>
        <a-sub-menu key="sub2">
          <template #title>
            <span>
              <team-outlined />
              <span>Team</span>
            </span>
          </template>
          <a-menu-item key="6">Team 1</a-menu-item>
          <a-menu-item key="8">Team 2</a-menu-item>
        </a-sub-menu>
        <a-menu-item key="9">
          <file-outlined />
          <span>File</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <header-component />
      <a-layout style="padding: 0 2rem">
        <router-view />
      </a-layout>
    </a-layout>
  </a-layout>
</template>
<script lang="ts">
import HeaderComponent from "./components/HeaderComponent.vue";
import {
  HomeOutlined,
  DesktopOutlined,
  UserOutlined,
  TeamOutlined,
  FileOutlined,
} from "@ant-design/icons-vue";
import { defineComponent, ref } from "vue";
export default defineComponent({
  components: {
    HomeOutlined,
    DesktopOutlined,
    UserOutlined,
    TeamOutlined,
    FileOutlined,
    HeaderComponent,
  },
  data() {
    return {
      collapsed: ref<boolean>(false),
      selectedKeys: ref<string[]>(["1"]),
    };
  },
});
</script>
<style>
#components-layout-demo-side .logo {
  height: 32px;
  margin: 16px;
  background: rgba(255, 255, 255, 0.3);
}

.site-layout .site-layout-background {
  background: #fff;
}
[data-theme="dark"] .site-layout .site-layout-background {
  background: #141414;
}
</style>
