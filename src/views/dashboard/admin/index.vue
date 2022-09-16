<!--
 * @Date: 2022-09-16 21:09:23
 * @LastEditors: CZH
 * @LastEditTime: 2022-09-17 03:32:03
 * @FilePath: /vue-ts-Admin/src/views/dashboard/admin/index.vue
-->
<template>
  <span>
    <div class="dashboard-editor-container">
      <div class="title">
        <i class="el-icon-s-promotion" style="margin-right: 10px" />
        能效分析
      </div>
      <el-row style="padding: 16px 16px 0">
        <el-col :span="16">
          <el-form
            :inline="true"
            :size="'mini'"
            ref="form"
            label-position="left"
            label-width="40px"
            @submit.stop="() => {}"
          >
            <el-form-item label="系统" style="color: white">
              <el-select v-model="name" placeholder="请选择">
                <el-option :value="'冷源主机'">冷源主机</el-option>
                <el-option :value="'空数据'">空数据</el-option>
                <el-option :value="'空数据'">空数据</el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="时间" style="color: white">
              <el-radio-group v-model="time">
                <el-radio-button label="当日"></el-radio-button>
                <el-radio-button label="7日"></el-radio-button>
                <el-radio-button label="30日"></el-radio-button>
                <el-radio-button label="自定义"></el-radio-button>
              </el-radio-group>
            </el-form-item>
          </el-form>
          <el-table :data="tableData" height="320" border style="width: 100%">
            <el-table-column prop="name" label="设备"> </el-table-column>
            <el-table-column prop="powerValue" label="性能"> </el-table-column>
            <el-table-column prop="limitValue" label="限值"> </el-table-column>
          </el-table>
        </el-col>
        <el-col :span="8">
          <chart style="width: 100%; height: 300px"></chart>
          <div class="btnWord">说明：冷站COP评价标准依据ASHRAE标准以及行业经验确定</div>
          <div class="rightWord">
            <span>评价标准</span>
            <div class="flexarround">
              <span class="fucksmall">>1.3</span>
              <span>待改善</span>
            </div>
            <div class="flexarround">
              <span class="fucksmall">0.9~1.3</span>
              <span>一般</span>
            </div>
            <div class="flexarround">
              <span class="fucksmall">0.39～0.9</span>
              <span>良好</span>
            </div>

            <div class="flexarround">
              <span class="fucksmall"> 小于0.39</span>
              <span>优秀</span>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
    <div class="dashboard-editor-container">
      <div class="title">
        <i class="el-icon-s-promotion" style="margin-right: 10px" />
        能效分析
      </div>
      <el-row style="padding: 16px 16px 0">
        <!-- <line-chart :chart-data="lineChartData" />
         -->
        <el-form
          :inline="true"
          :size="'mini'"
          ref="form"
          label-position="left"
          label-width="40px"
          @submit.stop="() => {}"
        >
          <el-radio-group v-model="power">
            <el-radio-button label="制冷性能能效比"></el-radio-button>
            <el-radio-button label="冷机性能系数"></el-radio-button>
            <el-radio-button label="冷冻水输送系数"></el-radio-button>
            <el-radio-button label="冷却水输送系数"></el-radio-button>
          </el-radio-group>
        </el-form>
        <br />
        <lineCharttt v-if="power == '制冷性能能效比'"></lineCharttt>
        <barChart v-else></barChart>
      </el-row>
    </div>
  </span>
</template>

<script lang="ts">
import * as echarts from "echarts";
import "echarts/theme/macarons.js"; // Theme used in BarChart, LineChart, PieChart and RadarChart
import { Component, Vue } from "vue-property-decorator";
import GithubCorner from "@/components/GithubCorner/index.vue";
import BarChart from "./components/BarChart.vue";
import BoxCard from "./components/BoxCard.vue";
import LineChart, { ILineChartData } from "./components/LineChart.vue";
import chart from "@/components/Charts/chart.vue";
import PanelGroup from "./components/PanelGroup.vue";
import PieChart from "./components/PieChart.vue";
import RadarChart from "./components/RadarChart.vue";
import TodoList from "./components/TodoList/index.vue";
import TransactionTable from "./components/TransactionTable.vue";
import lineCharttt from /* webpackChunkName: "line-chart" */ "@/views/charts/line-chart.vue";
import barChart from "@/views/charts/bar-chart.vue";

const lineChartData: { [type: string]: ILineChartData } = {
  newVisitis: {
    expectedData: [100, 120, 161, 134, 105, 160, 165],
    actualData: [120, 82, 91, 154, 162, 140, 145],
  },
  messages: {
    expectedData: [200, 192, 120, 144, 160, 130, 140],
    actualData: [180, 160, 151, 106, 145, 150, 130],
  },
  purchases: {
    expectedData: [80, 100, 121, 104, 105, 90, 100],
    actualData: [120, 90, 100, 138, 142, 130, 130],
  },
  shoppings: {
    expectedData: [130, 140, 141, 142, 145, 150, 160],
    actualData: [120, 82, 91, 154, 162, 140, 130],
  },
};

const fuckdatacell = {
  name: "CH07",
  powerValue: 3.5,
  limitValue: 9.2,
};
@Component({
  name: "DashboardAdmin",
  components: {
    GithubCorner,
    BarChart,
    barChart,
    BoxCard,
    lineCharttt,
    chart,
    LineChart,
    PanelGroup,
    PieChart,
    RadarChart,
    TodoList,
    TransactionTable,
  },
})
export default class extends Vue {
  private tableData = [
    fuckdatacell,
    fuckdatacell,
    fuckdatacell,
    fuckdatacell,
    fuckdatacell,
    fuckdatacell,
    fuckdatacell,
    fuckdatacell,
  ] as { [key: string]: any }[];

  private name = "冷源主机";
  private time = "当日";
  private power = "制冷性能能效比";

  private lineChartData = lineChartData.newVisitis;
  private handleSetLineChartData(type: string) {
    this.lineChartData = lineChartData[type];
  }
}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  background-color: $cardBackgroundColor;
  margin-bottom: $normalMargin;
  padding: 32px;
  position: relative;
  color: white;

  .title {
    height: 50px;
    line-height: 50px;
    width: calc(100% + 64px);
    border-bottom: rgba(255, 255, 255, 0.1) 1px solid;
    margin: -32px;
    padding-left: 32px;
    margin-bottom: 32px;
    font-size: 18px;
    font-weight: 300;
  }
  .github-corner {
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

.btnWord {
  width: 80%;
  margin: 0px 10%;
}

.rightWord {
  width: 25%;
  height: 300px;
  margin-top: -250px;
  margin-left: 75%;
  float: none;
  .flexarround {
    .fucksmall {
      color: rgba(255, 255, 255, 0.6);
    }
    margin-top: 10px;
    display: flex;
    justify-content: space-between;
    // border-bottom: 2px
    //   linear-gradient(rgba(0, 0, 0, 0), rgba(255, 255, 255, 1), rgba(0, 0, 0, 0)) solid;
    border-bottom: 1px rgba(255, 255, 255, 0.2) solid;
    padding: 3px 0px ;
  }
}
@media (max-width: 1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}
</style>
