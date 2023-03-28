<template>
  <div class="home">
    <el-container>
      <el-header>拍卖名称+参与状态</el-header>
      <el-container>
        <el-aside width="450px">意愿竞拍所得表格区域</el-aside>
        <el-main>
          <div id="details">实验详情区域</div>
          <div id="buttons">参与报价按键区域</div>
          <div id="charts">供需与价格曲线区域</div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
}
</script>

<style scoped>
.el-header {
  background-color: #B3C0D1;
  color: #333;
  text-align: center;
  line-height: 60px;
}
.el-aside {
  background-color: #D3DCE6;
  color: #333;
  text-align: center;
  line-height: 200px;
}
.el-main {
  background-color: #E9EEF3;
  color: #333;
  text-align: center;
  line-height: 700px;
}
#details{
  line-height: 200px;
  background-color: #d1a5a5;
}
#buttons{
  line-height: 80px;
  background-color: #B3C0D1;
}
#charts{
  line-height: 430px;
  background-color: antiquewhite;
}
</style>
