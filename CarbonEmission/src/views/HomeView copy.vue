<template>
  <div class="home">
    <el-container>
      <el-header>拍卖名称+参与状态</el-header>
      <el-container>
        <el-aside width="450px">
          <h3>意愿竞拍所得表格区域</h3>
          <el-table :data="marketData" style="width: 100%">
            <el-table-column prop="seller" label="卖家"></el-table-column>
            <el-table-column prop="buyer" label="买家"></el-table-column>
            <el-table-column prop="price" label="价格"></el-table-column>
          </el-table>
          <h3>我的交易记录</h3>
          <el-table :data="myData" style="width: 100%">
            <el-table-column prop="seller" label="卖家"></el-table-column>
            <el-table-column prop="buyer" label="买家"></el-table-column>
            <el-table-column prop="price" label="价格"></el-table-column>
          </el-table>
        </el-aside>
        <el-main>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import * as echarts from "echarts";

export default {
  name: "HomeView",
  data() {
    return {
      marketData: [
        { seller: "卖家1", buyer: "买家1", price: 100 },
        { seller: "卖家2", buyer: "买家3", price: 90 },
        { seller: "卖家3", buyer: "买家2", price: 80 },
      ],
      myData: [
        { seller: "卖家1", buyer: "买家1", price: 100 },
        { seller: "卖家2", buyer: "买家3", price: 90 },
      ],
    };
  },
  mounted() {
  
  },

  methods: {
   
  },
};
</script>

<style scoped>
.el-header {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
}
.el-aside {
  background-color: #d3dce6;
  color: #333;
  text-align: center;
  line-height: 200px;
}
.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
  line-height: 700px;
}
#details {
  line-height: 10px;
}
#buttons {
  line-height: 80px;
  background-color: #b3c0d1;
}
#charts {
  line-height: 500px;
  background-color: antiquewhite;
}
#maiform >>> .el-form-item__label {
  font-size: 15px;
}
#sellform >>> .el-form-item__label {
  font-size: 15px;
}
.costsell {
  margin-right: 200px;
}
.el-descriptions__body {
  line-height: 430px;
}
</style>
