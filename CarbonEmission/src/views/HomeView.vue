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
          <div id="charts">
            <div class="echart" id="mychart" :style="myChartStyle"></div>
<!--            供需与价格曲线区域-->
          </div>
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
      browseTime: 0,
      maiform: false,
      maiformtext: {
        number: "",
        cost: "",
      },
      mainumberarray: [],
      maicostarray: [],
      maiinfo: [],

      sellform: false,
      sellformtext: {
        number: "",
        cost: "",
      },
      sellnumberarray: [],
      sellcostarray: [],
      sellinfo: [],
      result:[],

      myChart:{},
      myChartStyle: { float: "left", width: "100%", height: "400px" } //图表样式
    };
  },
  mounted() {
    this.setTime(); // 页面加载完成后开始计时
    this.initEcharts();
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
        this.initEcharts();
        //console.log(this.browseTime, "时长累计");
      }, 1000);
    },
    maievent() {
      this.maiform = false;
      this.mainumberarray.push(this.maiformtext.number);
      this.maicostarray.push(this.maiformtext.cost);
      let mai_obj = {
        num: this.maiformtext.number,
        cost: this.maiformtext.cost
      };
      this.maiinfo.push(mai_obj);
      this.calc_balancePoint();
      //console.log(this.mainumberarray);
      //console.log(this.maicostarray);
    },
    sellevent() {
      this.sellform = false;
      this.sellnumberarray.push(this.sellformtext.number);
      this.sellcostarray.push(this.sellformtext.cost);
      let sell_obj = {
        num: this.sellformtext.number,
        cost: this.sellformtext.cost
      };
      this.sellinfo.push(sell_obj);
      this.calc_balancePoint();
      //console.log(this.sellnumberarray);
      //console.log(this.sellcostarray);
    },
    merge_cost() {
      //合并供给和需求报价，并从小到大排序
      var setobj = new Set(this.maicostarray);
      for (var i = 0; i < this.sellcostarray.length; i++){
        setobj.add(this.sellcostarray[i]);
      }
      var cost = Array.from(setobj).sort(function (a, b) { return a - b });
      //把去重后的报价填入到result数组中
      //在这里或许可以加上交易公平判断，目前没写
      for (var j = 0; j < cost.length; j++){
        let res_obj = {
          value: cost[j],
          mai: 0,
          sell: 0,
          dist:0
        };
        this.result.push(res_obj);
      }
    },
    calc_balancePoint() {
      //需求方按照报价从高到低排序
      this.maiinfo.sort(function (a, b) { return b.cost - a.cost });
      //供给方按照报价从低到高排序
      this.sellinfo.sort(function (a, b) { return a.cost - b.cost });
      this.result.splice(0,this.result.length);
      this.merge_cost();
      //遍历result，计算供给方、需求方的供给量和需求量，存入result中
      for (var i = 0; i < this.result.length; i++){
        var value = this.result[i].value;
        //需求方：报价>=价格时进入市场
        for (var p_mai = 0; p_mai < this.maiinfo.length; p_mai++){
          if (this.maiinfo[p_mai].cost >= value)
            this.result[i].mai += this.maiinfo[p_mai].num;
          else
            break;
        }
        //供给方：报价<=价格时进入市场
        for (var p_sell = 0; p_sell < this.sellinfo.length; p_sell++){
          if (this.sellinfo[p_sell].cost <= value)
            this.result[i].sell += this.sellinfo[p_sell].num;
          else
            break;
        }
        this.result[i].dist = Math.abs(this.result[i].mai - this.result[i].sell);

      }
    },
    initEcharts() {
      var xData=[]
      var buy=[]
      var sell=[]
      /*var xData=this.sellnumberarray
      var buy=this.maicostarray
      var sell=this.sellcostarray*/
      this.calc_balancePoint()
      for (let i = 0; i < this.result.length; i++) {
        xData[i]=this.result[i].value
        buy[i]=this.result[i].mai
        sell[i]=this.result[i].sell
      }
      console.log("xData")
      console.log(xData)
      console.log("buy")
      console.log(buy)
      console.log("sell")
      console.log(sell)
      const option = {
        xAxis: {
          data: xData,
          name:"市场价格"
        },
        legend: { // 图例
          data: ["需求", "供给"],
          bottom: "0%"
        },
        yAxis: {
          name:"供给量/需求量"
        },
        series: [
          {
            name: "需求",
            data: buy,
            type: "line", // 类型设置为折线图
            label: {
              show: true,
              position: "top",
              textStyle: {
                fontSize: 16
              }
            }
          },
          {
            name: "供给",
            data: sell,
            type: "line", // 类型设置为折线图
            label: {
              show: true,
              position: "top",
              textStyle: {
                fontSize: 16
              }
            }
          }
        ]
      };
      this.myChart = echarts.init(document.getElementById("mychart"));
      this.myChart.setOption(option);
      //随着屏幕大小调节图表
      window.addEventListener("resize", () => {
        this.myChart.resize();
      });
    }
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
