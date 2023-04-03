<template>
  <div class="home">
    <el-container>
      <el-header>拍卖名称+参与状态</el-header>
      <el-container>
        <el-aside width="450px">意愿竞拍所得表格区域</el-aside>
        <el-main>
          <div id="details">
            <el-descriptions title="实验详情">
              <template slot="extra">
                <el-button type="primary" size="small" @click="restart"
                  >重新开始</el-button
                >
              </template>
              <el-descriptions-item label="当前实验进度"
                ><el-tag size="small">进行中</el-tag></el-descriptions-item
              >
              <el-descriptions-item label="实验进行时长"
                >{{ this.browseTime }}s</el-descriptions-item
              >
              <el-descriptions-item label="总的参与人数">{{
                mainumberarray.length + sellnumberarray.length
              }}</el-descriptions-item>
              <el-descriptions-item label="买家数量">{{
                mainumberarray.length
              }}</el-descriptions-item>
              <el-descriptions-item label="卖家数量">{{
                sellnumberarray.length
              }}</el-descriptions-item>
            </el-descriptions>
          </div>
          <div id="buttons">
            <el-button type="button" @click="maiform = true" class="costsell"
              >我是买家</el-button
            >
            <el-dialog title="此次买方信息" :visible.sync="maiform" width="450px">
              <el-form :model="maiformtext" id="maiform">
                <el-form-item label="购买数量">
                  <el-input
                    clearable
                    v-model.number="maiformtext.number"
                    style="width: 180px"
                    placeholder="请输入购买数量"
                  ></el-input>
                </el-form-item>
                <el-form-item label="购买价格">
                  <el-input
                    clearable
                    v-model="maiformtext.cost"
                    style="width: 180px"
                    placeholder="请输入购买价格"
                  ></el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="maiform = false">取 消</el-button>
                <el-button type="primary" @click="maievent">确 定</el-button>
              </div>
            </el-dialog>

            <el-button type="button" @click="sellform = true">我是卖家</el-button>
            <el-dialog title="此次卖方信息" :visible.sync="sellform" width="450px">
              <el-form :model="sellformtext" id="sellform">
                <el-form-item label="购买数量">
                  <el-input
                    clearable
                    v-model.number="sellformtext.number"
                    style="width: 180px"
                    placeholder="请输入卖出数量"
                  ></el-input>
                </el-form-item>
                <el-form-item label="卖出价格">
                  <el-input
                    clearable
                    v-model="sellformtext.cost"
                    style="width: 180px"
                    placeholder="请输入卖出价格"
                  ></el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="sellform = false">取 消</el-button>
                <el-button type="primary" @click="sellevent">确 定</el-button>
              </div>
            </el-dialog>
          </div>
          <div id="charts">供需与价格曲线区域</div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "HomeView",
  data() {
    return {
      browseTime: 0,
      maiform: false,
      maiformtext: {
        number: "",
        cost: "",
      },
      mainumberarray: [],
      maicostarray: [],

      sellform: false,
      sellformtext: {
        number: "",
        cost: "",
      },
      sellnumberarray: [],
      sellcostarray: [],
    };
  },
  mounted() {
    this.setTime(); // 页面加载完成后开始计时
  },

  methods: {
    restart() {
      this.browseTime = 0;
      this.sellnumberarray = [];
      this.sellcostarray = [];
      this.mainumberarray = [];
      this.maicostarray = [];
    },
    setTime() {
      //设置定时器
      this.clearTimeSet = setInterval(() => {
        this.browseTime++;
        console.log(this.browseTime, "时长累计");
      }, 1000);
    },
    maievent() {
      this.maiform = false;
      this.mainumberarray.push(this.maiformtext.number);
      this.maicostarray.push(this.maiformtext.cost);
      console.log(this.mainumberarray);
      console.log(this.maicostarray);
    },
    sellevent() {
      this.sellform = false;
      this.sellnumberarray.push(this.sellformtext.number);
      this.sellcostarray.push(this.sellformtext.cost);
      console.log(this.sellnumberarray);
      console.log(this.sellcostarray);
    },
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
