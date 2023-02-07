<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  let xLabel = ['2018', '2019', '2020', '2021', '2022', '2023'];
  let goToSchool = ['40', '60', '22', '85', '50', '40'];

  const option = {
    backgroundColor: '#0e1c47',
    legend: {
      show: false
    },
    grid: {
      top: '15%',
      left: '10%',
      right: '10%',
      bottom: '15%'
      // containLabel: true
    },
    xAxis: [
      {
        type: 'category',
        boundaryGap: false,
        axisLine: {
          //坐标轴轴线相关设置。数学上的x轴
          show: true,
          lineStyle: {
            color: '#233653'
          }
        },
        axisLabel: {
          //坐标轴刻度标签的相关设置
          textStyle: {
            color: '#7ec7ff',
            padding: 16,
            fontSize: 14
          },
          formatter: function (data) {
            return data;
          }
        },
        splitLine: {
          show: false,
          lineStyle: {
            color: '#192a44'
          }
        },
        axisTick: {
          show: false
        },
        data: xLabel
      }
    ],
    yAxis: [
      {
        nameTextStyle: {
          color: '#7ec7ff',
          fontSize: 16,
          padding: 10
        },
        min: 0,
        splitLine: {
          show: true,
          lineStyle: {
            color: '#192a44'
          }
        },
        axisLine: {
          show: true,
          lineStyle: {
            color: '#233653'
          }
        },
        axisLabel: {
          show: true,
          textStyle: {
            color: '#7ec7ff',
            padding: 16
          },
          formatter: function (value) {
            if (value === 0) {
              return value;
            }
            return value;
          }
        },
        axisTick: {
          show: false
        }
      }
    ],
    series: [
      {
        type: 'line',
        symbol: 'circle', // 默认是空心圆（中间是白色的），改成实心圆
        showAllSymbol: true,
        symbolSize: 0,
        smooth: true,
        lineStyle: {
          normal: {
            width: 2,
            color: 'rgba(131,238,157,1)' // 线条颜色
          },
          borderColor: 'rgba(0,0,0,.4)'
        },
        itemStyle: {
          color: 'rgba(131,238,157,1)',
          borderColor: '#646ace',
          borderWidth: 2
        },
        tooltip: {
          show: true
        },
        areaStyle: {
          //区域填充样式
          normal: {
            //线性渐变，前4个参数分别是x0,y0,x2,y2(范围0~1);相当于图形包围盒中的百分比。如果最后一个参数是‘true’，则该四个值是绝对像素位置。
            color: new echarts.graphic.LinearGradient(
              0,
              0,
              0,
              1,
              [
                {
                  offset: 0,
                  color: 'rgba(131,238,157,.3)'
                },
                {
                  offset: 1,
                  color: 'rgba(131,238,157, 0)'
                }
              ],
              false
            ),
            shadowColor: 'rgba(131,238,157, 0.5)', //阴影颜色
            shadowBlur: 20 //shadowBlur设图形阴影的模糊大小。配合shadowColor,shadowOffsetX/Y, 设置图形的阴影效果。
          }
        },
        data: goToSchool
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
