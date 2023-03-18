<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  const option = {
    backgroundColor:'rgba(011, 023, 059)',
    tooltip: {
      trigger: 'axis',
      axisPointer: { // 坐标轴指示器，坐标轴触发有效
        type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
      },
      formatter: function(list) {
        var msg = "";
        for (let i in list) {
          if (i == 0) {
            msg += list[i].name + "<br>";
          }
          msg += list[i].seriesName + "：" + list[i].data + "万元<br>";
          if (i > 0 && list[i].seriesName == "不可用余额" && list[i - 1].seriesName == "可用余额") {
            msg += "总存款余额" + "：" + (list[i].data + list[i - 1].data) + "万元<br>";
          }
          if (i > 0 && list[i].seriesName == "支出" && list[i - 1].seriesName == "收入") {
            msg += "净收入" + "：" + (list[i - 1].data - list[i].data) + "万元<br>";
          }
        }
        return msg;
      }
    },
    legend: {
      textStyle: {
        color: '#fff',
      },
      data: ['可用余额', '不可用余额', '收入', '支出']
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '3%',
      containLabel: true
    },
    xAxis: {
      type: 'category',
      axisLine: {
        lineStyle: {
          color: '#808eb7',
          width: 2
        }
      },
      data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日'],
    },
    yAxis: {
      axisLine: {
        lineStyle: {
          color: '#808eb7',
          width: 2
        }
      },
      splitLine: { //分割线配置
        lineStyle: {
          color: "#AAAAAA56",
        }
      },
    },
    series: [{
      name: '可用余额',
      type: 'bar',
      stack: '余额',
      barMaxWidth: 30,
      data: [120, 132, 101, 134, 90, 230, 210],
      itemStyle: {
        normal: {
          color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [{
            offset: 1,
            color: "#00ffff" // 0% 处的颜色
          },
            {
              offset: 0,
              color: "#3893e5" // 100% 处的颜色
            }
          ], false),
        },
      },
    },{
      name: '可用余额',
      type: 'bar',
      stack: '余额1',
      barMaxWidth: 30,
      data: [120, 132, 101, 134, 90, 230, 210],
      itemStyle: {
        normal: {
          color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [{
            offset: 1,
            color: "#094949" // 0% 处的颜色
          },
            {
              offset: 0,
              color: "#3893e5" // 100% 处的颜色
            }
          ], false),
        },
      },
    },
      {
        name: '不可用余额',
        type: 'bar',
        stack: '余额',
        barMaxWidth: 30,
        data: [220, 182, 191, 234, 290, 330, 310],
        itemStyle: {
          normal: {
            borderWidth: 2,
            borderColor: '#00ffff',
            color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [{
              offset: 1,
              color: "#bab3bd69" // 0% 处的颜色
            },
              {
                offset: 0,
                color: "#bab3bd69" // 100% 处的颜色
              }
            ], false),
          },
        },
      },
      {
        name: '不可用余额',
        type: 'bar',
        stack: '余额1',
        barMaxWidth: 30,
        data: [220, 182, 191, 234, 290, 330, 310],
        itemStyle: {
          normal: {
            borderWidth: 2,
            borderColor: '#094949',
            color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [{
              offset: 1,
              color: "#bab3bd69" // 0% 处的颜色
            },
              {
                offset: 0,
                color: "#bab3bd69" // 100% 处的颜色
              }
            ], false),
          },
        },
      },
    ]
  };
  myChart.setOption(option);
};

nextTick(async () => {
  initChart();
});
</script>

<template>
  <div id="main" class="w-[640px] h-[400px]"></div>
</template>

<style>
#main {
  text-align: center;
  border-radius: 20px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>

<route lang="yml">
meta:
  layout: blank
</route>
