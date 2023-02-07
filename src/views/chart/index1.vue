<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  var data1 = [500, 356, 896, 650];
  var xData = ['正高', '副高', '中级', '初级'];
  const option = {
    backgroundColor: '#020D20',
    grid: {
      borderWidth: 0,
      top: '20%',
      left: '65',
      bottom: '25%',
      textStyle: {
        color: '#fff'
      }
    },
    legend: {
      show: false
    },
    xAxis: [
      {
        type: 'category',
        axisLine: {
          lineStyle: {
            color: 'rgba(255,255,255,.5)'
          }
        },
        splitLine: {
          show: false
        },
        axisTick: {
          show: false
        },
        splitArea: {
          show: false
        },
        axisLabel: {
          interval: 0,
          color: 'rgba(255,255,255,0.7)',
          fontSize: 18
        },
        data: xData
      }
    ],
    yAxis: {
      type: 'value',
      splitLine: {
        show: true,
        lineStyle: {
          color: 'rgba(135,140,147,1)' //左侧显示线
        }
      },
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        interval: 0,
        color: 'rgba(255,255,255,0.5)',
        fontSize: 20
      },
      splitArea: {
        show: false
      }
    },
    series: [
      {
        // name: '周转量',
        type: 'bar',
        // "stack": "总量",
        barMaxWidth: 35,
        barGap: '10%',
        itemStyle: {
          normal: {
            color: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#05F5FF' // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: '#020D20' // 100% 处的颜色
                }
              ],
              global: false // 缺省为 false
            }
          }
        },
        label: {
          normal: {
            show: true,
            position: 'top',
            color: '#fff'
          }
        },
        data: data1
      }
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
