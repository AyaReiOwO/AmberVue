<script setup>
import * as echarts from 'echarts';

const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  let xData = ['二级','三级','四级','', '五级','六级','七级','', '八级','九级','十级','其他'];
  let data = [
    {
      name: '高级',
      value: [14, 0, 0, 0,   0, 0, 0, 0   , 0, 0 , 0, 0]
    },
    {
      name: '高级',
      value: [0, 13, 0, 0, 0, 0, 0, 0 , 0, 0 , 0, 0]
    },
    {
      name: '高级',
      value: [0, 0, 38, 0, 0, 0, 0, 0 , 0, 0 , 0, 0]
    },
    {
      name: '高级',
      value: [0, 0, 0, 0, 0, 0, 0, 0 , 0, 0 , 0, 0]
    },
    {
      name: '副高级',
      value: [0, 0, 0, 0, 23, 0, 0, 0, 0, 0 , 0, 0]
    },
    {
      name: '副高级',
      value: [0, 0, 0, 0,0, 20, 0, 0, 0, 0 , 0, 0]
    },
    {
      name: '副高级',
      value: [0, 0, 0, 0, 0,0, 26, 0,0, 0 , 0, 0]
    },
    {
      name: '副高级',
      value: [0, 0, 0, 0, 0, 0, 0, 0 , 0, 0 , 0, 0]
    },
    {
      name: '中级及其他',
      value: [0, 0, 0, 0, 0,0, 0, 0, 7, 0 , 0, 0]
    },
    {
      name: '中级及其他',
      value: [0, 0, 0, 0, 0,0, 0, 0, 0, 2 , 0, 0]
    },
    {
      name: '中级及其他',
      value: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0 , 8, 0]
    },
    {
      name: '中级及其他',
      value: [0, 0, 0, 0, 0, 0,0, 0, 0, 0 , 0,1]
    },
    // {
    //   name: '副高级',
    //   value: [ 0, 0, 0  , 7, 15, 21, 0, 0, 0],
    // },
    // {
    //   name: '中级及其他',
    //   value: [0, 0, 0, 0, 0, 0  , 17, 15, 13],
    // },
  ];

  const color = [
    new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
      offset: 0,
      color: '#3A89FE'
    }, {
      offset: 1,
      color: '#6CBEFF00'
    }]),
    new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
      offset: 0,
      color: '#4CD964'
    }, {
      offset: 1,
      color: '#007a5500'
    }]),
    new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
      offset: 0,
      color: '#00F6F9'
    }, {
      offset: 1,
      color: '#00EAF100'
    }]),
    new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
      offset: 0,
      color: '#F1C40F'
    }, {
      offset: 1,
      color: '#F9E42200'
    }]),
  ]

  const legendData = []

  const seriesData = data.map((item,index) => {
    item.value = item.value.map((_item,_index)=>{
      legendData.push({
        name:item.name,
        itemStyle: {
          color: color[parseInt((index/4)) % color.length],
        },
      })
      return {
        name: xData[index],
        value:_item ,
        itemStyle: {
          normal: {
            color: color[parseInt((_index/4)) % color.length],
            opacity: 1,
          }
        },
      }
    })
    return {
      // name: legendData[index],
      name:item.name,
      type: 'bar',
      data: item.value,
      barWidth: 16, //柱子宽度
      barGap: 1, //柱子之间间距
      stack: '余额',
      label: {
        show: true,
        position: 'top',
        fontFamily: 'pmzd',
        fontSize: 14,
        fontWeight: 600,
        lineHeight: 16,
        textShadowColor: '#3A89FE',
        textShadowOffsetX: 0,
        textShadowOffsetY: 0,
        textShadowBlur: 9,
        formatter: function (params) {
          // 假设此轴的 type 为 'time'。
          if(params.value){
            return params.value;
          }else{
            return '';
          }
        },
      }
    }
  })


  const option = {
    backgroundColor: '#000000',
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow'
      }
    },
    legend: {
      data: [
        ...legendData,
        {
          name:'职称占比',
          icon:'image://data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAICAYAAADJEc7MAAAAAXNSR0IArs4c6QAAAJhJREFUKFOF0b8NgkAUx/HvD6g19mdC5QwOYGWpK2iYwQmc4aIraGnlAM5gZSK90Rp4BrFSDqrLy+Xz/grA8vWEItoCszoGziTVRm53/cZ/jxqkCzD6+X2Q2DSEZbfsALZAOhGX2QeXscdsDjoq9cu2qjV8gg1IqrHcPm9aXzmK6A56KfXDVhiaoTdhB+weIQj7lhaCfWd6A6BgTqG3z62gAAAAAElFTkSuQmCC',
        }
      ],
      // align: 'center',
      bottom: 10,
      textStyle: {
        color: "#fff",
      },
      itemWidth: 10,
      itemHeight: 10,
      // itemGap: 35
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '10%',
      containLabel: true
    },
    xAxis: [{
      type: 'category',
      data: xData,
      axisLine: {
        show: false,
        lineStyle: {
          color: "#00c7ff40",
          width: 1,
          type: "solid"
        }
      },
      axisTick: {
        show: false,
      },
      axisLabel: {
        show: true,
        color:'#99C2FF',
        fontFamily:'pmzd',
        fontSize: 14,
        fontWeight: 500,
        lineHeight: 16,
        textShadowColor:'rgba(0,102,255,0.7)',
        textShadowOffsetX:0,
        textShadowOffsetY:0,
        textShadowBlur:18,
      },
    },{
      type: 'category',
      data: ['高级','副高级','中级及其他'],
      axisLine: {
        show: false,
        lineStyle: {
          color: "#00c7ff40",
          width: 1,
          type: "solid"
        }
      },
      axisTick: {
        show: false,
      },
      axisLabel: {
        show: false,
        color:'#99C2FF',
        fontFamily:'pmzd',
        fontSize: 14,
        fontWeight: 500,
        lineHeight: 16,
        textShadowColor:'rgba(0,102,255,0.7)',
        textShadowOffsetX:0,
        textShadowOffsetY:0,
        textShadowBlur:18,
      },
    }],
    yAxis: [{
      type: 'value',
      axisLabel: {
        color: '#ffffff99',
        formatter: '{value}'
      },
      axisTick: {
        show: false,
      },
      axisLine: {
        show: false,
        lineStyle: {
          color: "#00c7ff10",
          width: 1,
          // type: "solid"
        },
      },
      splitLine: {
        lineStyle: {
          color: "#00c7ff40",
        }
      }
    },{
      type: 'value',
      axisLabel: {
        color: '#ffffff99',
        formatter: '{value}%'
      },
      axisTick: {
        show: false,
      },
      axisLine: {
        show: false,
        lineStyle: {
          color: "#00c7ff10",
          width: 1,
          // type: "solid"
        },
      },
      splitLine: {
        show:false,
        lineStyle: {
          color: "#00c7ff40",
        }
      }
    }],
    series: [
        ...seriesData,
      {
        data: [43,45,12],
        yAxisIndex: 1,
        xAxisIndex: 1,
        type: 'line',
        // smooth: true,
        name: '职称占比',
        itemStyle:{
          color:'#FED019'
        },
        label:{
          show:true,
          color:'#FED019',
          formatter: '{c}%'
        },
        symbolSize: 2 // 控制线条上 点 的大小
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
  <div id="main" className="w-[640px] h-[400px]"></div>
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
