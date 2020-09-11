<template>

  <div class="app-container documentation-container">
    <el-tabs :tab-position="tabPosition" style="height: 500px;">
      <el-tab-pane label="菜鸟教程">
        <iframe style="width:1000px;height:1500px;" src="https://pan.baidu.com/s/1CliU4XgtqFQ6YhLkUHDQyA" />
      </el-tab-pane>
      <el-tab-pane label="w3school">
        <iframe style="width:1000px;height:1500px;" src="https://www.w3school.com.cn/index.html" />
      </el-tab-pane>
      <el-tab-pane label="MDN">
        <iframe style="width:1000px;height:1500px;" src="https://developer.mozilla.org/zh-CN/" />
      </el-tab-pane>
      <el-tab-pane label="微信小程序">
        <iframe style="width:1000px;height:1500px;" src="https://developers.weixin.qq.com/miniprogram/dev/framework/" />
      </el-tab-pane>
    </el-tabs>
    <!--
     <a class="document-btn" target="_blank" href="https://oursparkspace.cn">火花空间</a>
     <a class="document-btn" target="_blank" href="https://pan.baidu.com/s/1CliU4XgtqFQ6YhLkUHDQyA">菜鸟教程</a>
     <a class="document-btn" target="_blank" href="https://www.w3school.com.cn/index.html">w3school</a>
     <a class="document-btn" target="_blank" href="https://developer.mozilla.org/zh-CN/">MDN</a>
     <a class="document-btn" target="_blank" href="https://developers.weixin.qq.com/miniprogram/dev/framework/">微信小程序</a>
     -->
     
  </div>
  
</template>


<script>
import DropdownMenu from '@/components/Share/DropdownMenu'
import axios from 'axios'
export default {
  name: 'Documentation',
  components: { DropdownMenu },
   data() {
      return {
        tabPosition: 'left'
      };
    }
}
</script>

<style lang="scss" scoped>
.documentation-container {
  margin: 50px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;

  .document-btn {
    flex-shrink: 0;
    display: block;
    cursor: pointer;
    background: black;
    color: white;
    height: 60px;
    width: 200px;
    margin-bottom: 16px;
    line-height: 60px;
    font-size: 20px;
    text-align: center;
  }
}
</style>
