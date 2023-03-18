<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  let xData = ['武汉理工大学', '清华大学', '清华大学', '北京航空航天大学', '西北工业大学', '浙江大学'];
  let data = [
    {
      name:'国际知名学者',
      value:[15,14,12,12,3,8]
    },
    {
      name:'青年拔尖英才',
      value:[7,15,21,7,11,16],
    },
    {
      name:'中年领军专家',
      value:[17,15,13,21,22,16],
    },
    {
      name:'资深学术权威',
      value:[5,4,2,2,2,3],
    },
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
      color: '#F1C40F'
    }, {
      offset: 1,
      color: '#F9E42200'
    }]),
    new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
      offset: 0,
      color: '#00F6F9'
    }, {
      offset: 1,
      color: '#00EAF100'
    }]),
  ]

  const legendData = data.map(item=>item.name)

  const seriesData = data.map((item,index)=>{
    return {
      name: legendData[index],
      type: 'bar',
      data: item.value,
      barWidth: 16, //柱子宽度
      barGap: 1, //柱子之间间距
      itemStyle: {
        normal: {
          color: color[index%color.length],
          opacity: 1,
        }
      },
      label:{
        show:true,
        position:'top',
        fontFamily:'pmzd',
        fontSize: 14,
        fontWeight: 600,
        lineHeight: 16,
        textShadowColor:'#3A89FE',
        textShadowOffsetX:0,
        textShadowOffsetY:0,
        textShadowBlur:9,
      }
    }
  })



  const option =  {
    backgroundColor: '#000000',
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow'
      }
    },
    legend: {
      data: legendData,
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
        color:'#fff',
        fontFamily:'pmzd',
        fontSize: 14,
        fontWeight: 500,
        lineHeight: 16,
        textShadowColor:'#3A89FE',
        textShadowOffsetX:0,
        textShadowOffsetY:0,
        textShadowBlur:9,
      },
    }],
    yAxis: [{
      type: 'value',
      axisLabel: {
        color:'#ffffff99',
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
    }],
    series: seriesData
  };

  myChart.setOption(option);
};

nextTick(async () => {
  initChart();
});
</script>

<template>
  <div id="main" class="w-[1200px] h-[400px]"></div>
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
