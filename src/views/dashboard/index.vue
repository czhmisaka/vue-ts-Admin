<!--
 * @Date: 2022-09-16 21:09:23
 * @LastEditors: CZH
 * @LastEditTime: 2022-09-17 01:28:19
 * @FilePath: /vue-ts-Admin/src/views/dashboard/index.vue
-->
<template>
  <div class="dashboard-container">
    <component :is="currentRole" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import { UserModule } from '@/store/modules/user'
import AdminDashboard from './admin/index.vue'
import EditorDashboard from './editor/index.vue'

@Component({
  name: 'Dashboard',
  components: {
    AdminDashboard,
    EditorDashboard
  }
})
export default class extends Vue {
  private currentRole = 'admin-dashboard'

  get roles() {
    return UserModule.roles
  }

  created() {
    if (!this.roles.includes('admin')) {
      this.currentRole = 'editor-dashboard'
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard-container{
  min-height: 100%;
  margin: $normalMargin;
}
</style>
