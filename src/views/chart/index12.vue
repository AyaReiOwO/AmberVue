<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  var data = [{
    value:35,
    rank:'95/269'
  }, {
    value:60,
    rank:'95/269'
  },{
    value:60,
    rank:'95/269'
  },{
    value:60,
    rank:'95/269'
  },{
    value:60,
    rank:'95/269'
  },{
    value:100,
    rank:'95/269'
  }, ];
  var xData = [
      '应用经济学', '马克思主义理论', '数学', '物理学', '化学', '力学', '机械工程', '仪器科学与技术',
      // '材料科学与工程', '动力工程及工程热物理', '信息与通信工程', '计算机科学与技术', '土木工程', '矿业工程', '交通运输工程', '船舶与海洋工程',
      // '环境科学与工程', '安全科学与工程', '管理科学与工程', '工商管理（原为工商管理学）', '艺术学理论（原为艺术学）', '设计学',
  ];
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
            color: 'rgba(255,255,255,.2)'
          }
        },
        splitLine: {
          show: false,
        },
        axisTick: {
          show: false
        },
        splitArea: {
          show: false
        },
        axisLabel: {
          interval: 0,
          color: '#ffffff80',
          fontSize: 12,
          fontWeight:600,
        },
        data: xData
      }
    ],
    yAxis: {
      type: 'value',
      splitLine: {
        show: true,
        lineStyle: {
          color: 'rgba(135,140,147,0.3)' //左侧显示线
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
        color: '#ffffff80',
        fontSize: 12,
        fontWeight:600,
        formatter: (params) => {
          console.log(params)
          if(params === 0){
            return  '>70%'
          }else{
            return `前${100 - params}`
          }

        }

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
                  color: `#2E9DF1` // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: `#2E9DF130` // 100% 处的颜色
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
            color: '#fff',
            formatter: (params) => {
              return (
                  "{title|" + params.data.rank + "}{value| }"
              );
            },
            textStyle: {
              rich: {
                title: {
                  color: 'rgba(250,220,102,0.9)',
                  fontSize:14,
                  fontFamily:'MicrosoftYaHei',
                  fontWeight:600,
                  lineHeight: 19,
                  textShadowColor:'rgba(250,220,102,0.9)',
                  textShadowOffsetX:0,
                  textShadowOffsetY:0,
                  textShadowBlur:8,
                },
                value: {
                  width: 14,
                  height: 14,
                  align:'left',
                  color: 'white',
                  fontSize:20,
                  lineHeight: 19,
                  fontWeight:600,
                  fontFamily:'pmzd',
                  textShadowColor:'#3A89FE',
                  textShadowOffsetX:0,
                  textShadowOffsetY:0,
                  textShadowBlur:9,
                  backgroundColor: {
                    image: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAICAYAAADJEc7MAAAAAXNSR0IArs4c6QAAAJhJREFUKFOF0b8NgkAUx/HvD6g19mdC5QwOYGWpK2iYwQmc4aIraGnlAM5gZSK90Rp4BrFSDqrLy+Xz/grA8vWEItoCszoGziTVRm53/cZ/jxqkCzD6+X2Q2DSEZbfsALZAOhGX2QeXscdsDjoq9cu2qjV8gg1IqrHcPm9aXzmK6A56KfXDVhiaoTdhB+weIQj7lhaCfWd6A6BgTqG3z62gAAAAAElFTkSuQmCC'
                    // 这里可以是图片的 URL，
                    // 或者图片的 dataURI，
                    // 或者 HTMLImageElement 对象，
                    // 或者 HTMLCanvasElement 对象。
                  }
                }
              },
            },
          }
        },
        data: data
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
