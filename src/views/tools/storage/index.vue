<template>
  <el-tabs v-model="activeName" style="padding-left: 8px;" @tab-click="tabClick">
    <el-tab-pane label="本地存储" name="first">
      <Local ref="local" />
    </el-tab-pane>
    <el-tab-pane label="七牛云存储" name="second">
      <QiNiu ref="qiNiu" />
    </el-tab-pane>
    <el-tab-pane label="腾讯云cos存储" name="third">
      <Cos ref="cos" />
    </el-tab-pane>
  </el-tabs>
</template>

<script>
import QiNiu from './qiniu/index'
import Cos from './cos/index.vue'
import Local from './local/index'
export default {
  name: 'Storage',
  components: { QiNiu, Local, Cos },
  data() {
    return {
      activeName: 'first'
    }
  },
  methods: {
    tabClick(name) {
      if (this.activeName === 'first') {
        this.$refs.local.crud.toQuery()
      } else if (name === 'second') {
        this.$refs.qiNiu.crud.toQuery()
      } else if (name === 'third') {
        this.$refs.cos.crud.toQuery()
      }
    }
  }
}
</script>

<style scoped>
</style>
