<template>
  <div class="header-wapper">
    <a-input-search class="search-top" />

    <a-dropdown :trigger="['click']">
      <a-avatar
        :width="40"
        src="https://scontent.fhan4-3.fna.fbcdn.net/v/t1.18169-1/1384160_1393778980857992_1567848449_n.jpg?stp=c7.0.40.40a_cp0_dst-jpg_p40x40&_nc_cat=100&ccb=1-7&_nc_sid=7206a8&_nc_ohc=pu6BDp3sXT4AX-y5NEW&_nc_ht=scontent.fhan4-3.fna&oh=00_AfAkFSqcaldd6h-c--vbaSiRXgKS0v7WCZYDpYBxbyF3gg&oe=63D46E27"
        style="cursor: pointer"
        @click.prevent
      />
      <template #overlay>
        <a-menu>
          <a-menu-item key="1">
            <UserOutlined />
            Thông tin người dùng
          </a-menu-item>
          <a-menu-item key="2">
            <SettingOutlined />
            Cài đặt
          </a-menu-item>
          <a-menu-item key="3">
            <ExportOutlined />
            Đăng Xuất
          </a-menu-item>
        </a-menu>
      </template>
    </a-dropdown>
  </div>
</template>
<script lang="ts">
import {
  ExportOutlined,
  SettingOutlined,
  UserOutlined,
} from "@ant-design/icons-vue";
import { defineComponent, ref } from "vue";

export default defineComponent({
  setup() {},
  components: { UserOutlined, SettingOutlined, ExportOutlined },
});
</script>

<style lang="css">
.header-wapper {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  background-color: #40a9ff;
  min-height: 46px;
  padding: 0px 2rem;
}
.header-wapper .search-top {
  max-width: 300px;
  border-radius: 999px;
  margin-right: 24px;
}
</style>
