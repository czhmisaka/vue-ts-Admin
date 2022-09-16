<template>
  <div :id="id" :class="className" :style="{ height: height, width: width }" />
</template>

<script lang="ts">
import * as echarts from "echarts";
import { Component, Prop } from "vue-property-decorator";
import { mixins } from "vue-class-component";
import ResizeMixin from "./mixins/resize";

@Component({
  name: "LineChart",
})
export default class extends mixins(ResizeMixin) {
  @Prop({ default: "asd" }) private className!: string;
  @Prop({ default: "asd" }) private id!: string;
  @Prop({ default: "200px" }) private width!: string;
  @Prop({ default: "200px" }) private height!: string;

  mounted() {
    this.$nextTick(() => {
      this.initChart();
    });
  }

  beforeDestroy() {
    if (!this.chart) {
      return;
    }
    this.chart.dispose();
    this.chart = null;
  }

  private initChart() {
    this.chart = echarts.init(document.getElementById(this.id) as HTMLDivElement);
    let option = {
      series: [
        {
        center: ['40%', '55%'],
          type: "gauge",
          startAngle: 180,
          endAngle: 0,
          min: 0,
          max: 1,
          splitNumber: 8,
          axisLine: {
            lineStyle: {
              width: 6,
              color: [
                [0.25, "rgb(73,176,255)"],
                [0.5, "rgb(80,200,255)"],
                [0.75, "rgb(91,227,255	)"],
                [1, "rgb(93,255,255)"],
              ],
            },
          },
          pointer: {
            icon: "path://M12.8,0.7l12,40.1H0.7L12.8,0.7z",
            length: "12%",
            width: 20,
            offsetCenter: [0, "-60%"],
            itemStyle: {
              color: "auto",
            },
          },
          axisTick: {
            length: 12,
            lineStyle: {
              color: "auto",
              width: 2,
            },
          },
          splitLine: {
            length: 20,
            lineStyle: {
              color: "auto",
              width: 5,
            },
          },
          axisLabel: {
            color: "#fff",
            fontSize: 12,
            distance: -60,
            formatter: function (value: any) {
              if (value === 0.875) {
                return "优秀";
              } else if (value === 0.625) {
                return "良好";
              } else if (value === 0.375) {
                return "一般";
              } else if (value === 0.125) {
                return "带改善";
              }
              return "";
            },
          },
          title: {
            offsetCenter: [0, "30%"],
            color: "#07ccff",
            fontSize: 30,
          },
          detail: {
            fontSize: 20,
            offsetCenter: [0, "0%"],
            valueAnimation: true,
            formatter: function (value: any) {
              //   return Math.round(value * 100) + "分" ;
              return 0.76;
            },
            color: "auto",
          },
          data: [
            {
              value: 0.7,
              name: "良好",
            },
          ],
        },
      ],
    };

    this.chart.setOption(option);
  }
}
</script>
