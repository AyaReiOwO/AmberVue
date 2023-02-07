<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  const data = [
    {
      value: 45,
      name: '中级'
    },
    {
      value: 25,
      name: '初级'
    },
    {
      value: 15,
      name: '正高'
    },
    {
      value: 8,
      name: '副高'
    }
  ];

  const data2 = [
    {
      value: 40,
      name: '专科'
    },
    {
      value: 5,
      name: '硕士研究生'
    },
    {
      value: 25,
      name: '博士'
    },
    {
      value: 15,
      name: '本科'
    },
    {
      value: 8,
      name: '博士后'
    }
  ];

  const colorList = ['#145ded', '#45c761', '#27c6b1', '#148fee'];

  const colorList2 = [
    ['#145ded', '#1f457c'],
    ['#05bfcf', '#12578c'],
    ['#3bb175', '#185a8a'],
    ['#baab3c', '#275681'],
    ['#2667da', '#0c3872']
  ];

  const imageUrl1 =
    'data:image/png;base64,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';

  const option = {
    graphic: {
      elements: [
        {
          type: 'image',
          style: {
            image: imageUrl1,
            width: 56,
            height: 56
          },
          left: 'center',
          top: 'center',
          z: 99
        },
        {
          type: 'image',
          style: {
            image: imageUrl1,
            width: 200,
            height: 200
          },
          left: 'center',
          top: 'center'
        }
      ]
    },
    series: [
      {
        type: 'pie',
        radius: ['0%', '68%'],
        center: ['50%', '50%'],
        clockwise: false,
        data: data,
        label: {
          normal: {
            show: true,
            position: 'inner',
            color: '#fff'
          }
        },
        labelLine: {
          normal: {
            show: false
          }
        },
        itemStyle: {
          normal: {
            borderWidth: 0,
            borderColor: '#ffffff',
            color: function (params) {
              return `${colorList[params.dataIndex]}`;
            }
          },
          emphasis: {
            borderWidth: 0,
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        }
      },
      {
        type: 'pie',
        radius: ['68%', '72%'],
        center: ['50%', '50%'],
        clockwise: false,
        data: data,
        label: {
          normal: {
            show: false
          }
        },
        labelLine: {
          normal: {
            show: false
          }
        },
        itemStyle: {
          normal: {
            borderWidth: 0,
            borderColor: '#ffffff',
            color: function (params) {
              return `${colorList[params.dataIndex]}70`;
            }
          },
          emphasis: {
            borderWidth: 0,
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        }
      },
      {
        type: 'pie',
        radius: ['74%', '95%'],
        center: ['50%', '50%'],
        clockwise: false,
        data: data2,
        label: {
          normal: {
            show: true,
            color: '#fff'
          }
        },
        labelLine: {
          normal: {
            show: true,
            length: 15,
            length2: 20
          }
        },
        itemStyle: {
          normal: {
            borderWidth: 3,
            borderColor: '#0e1c47',
            color: function (params) {
              return new echarts.graphic.LinearGradient(
                0,
                0,
                0,
                1,
                [
                  {
                    offset: 0,
                    color: `${colorList2[params.dataIndex][0]}80`
                  },
                  {
                    offset: 1,
                    color: `${colorList2[params.dataIndex][1]}99`
                  }
                ],
                false
              );
            }
          },
          emphasis: {
            borderWidth: 0,
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        }
      },
      {
        type: 'pie',
        radius: ['0%', '75%'],
        center: ['50%', '50%'],
        clockwise: false,
        data: data,
        label: {
          normal: {
            show: false
          }
        },
        labelLine: {
          normal: {
            show: false
          }
        },
        itemStyle: {
          normal: {
            borderWidth: 3,
            borderColor: '#0e1c47',
            color: function (params) {
              return `${colorList[params.dataIndex]}00`;
            }
          },
          emphasis: {
            borderWidth: 0,
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        }
      }
    ],
    color: colorList,
    backgroundColor: '#0e1c47'
  };

  myChart.setOption(option);
};

nextTick(async () => {
  initChart();
});
</script>

<template>
  <div id="main" class="w-[320px] h-[200px]"></div>
</template>

<style>
#main {
  text-align: center;
  border-radius: 20px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: #000;
}
</style>

<route lang="yml">
meta:
  layout: blank
</route>
