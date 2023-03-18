<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  const color = [ '#00EAF1','#FFE066', '#FFE066']

 let  data = [{
    name: "专职辅导员全校占比",
    value: 36,
    i:'6.96%',
  },
    {
      name: "专职辅导员/学生人数",
      value: 500,
      i:'1:500',
    },
    {
      name: "专兼职辅导员/学生人数",
      value: 367,
      i:'1:367',
    },
  ];

  let  sumValue = 0 ;
  data = data.map((item,index)=>{
    sumValue = sumValue + item.value
    item.textStyle = {
      color:color[index]
    }
    return item
  })

  const seriesData = []
  data.forEach((item,index)=>{
    seriesData.push({
      name: '',
      type: 'pie',
      clockWise: false, //顺时加载
      hoverAnimation: false, //鼠标移入变大
      radius: [75 - index * 15 + '%',  65- index * 15 + '%'],
      center: ["50%", "55%"],
      label: {
        show: false
      },
      itemStyle: {
        label: {
          show: false,
        },
        labelLine: {
          show: false
        },
        borderWidth: 10,
      },
      data: [{
        value: data[index].value,
        name: data[index].name
      }, {
        value: sumValue - data[index].value,
        name: '',
        itemStyle: {
          color: "rgba(0,0,0,0)",
          borderWidth: 0
        },
        tooltip: {
          show: false
        },
        hoverAnimation: false
      }]
    });
    if(index=== 0){
      seriesData.push({
        name: '',
        type: 'pie',
        silent: true,
        z: 1,
        clockWise: false, //顺时加载
        hoverAnimation: false, //鼠标移入变大
        radius: [75 - index * 15 + '%',  65- index * 15 + '%'],
        center: ["50%", "55%"],
        label: {
          show: false
        },
        itemStyle: {
          opacity:0.8,
          label: {
            show: false,
          },
          labelLine: {
            show: false
          },
          borderWidth: 5,
        },
        data: [{
          value: 7.5,
          itemStyle: {
            color: "rgb(3, 31, 62)",
            borderWidth: 0
          },
          tooltip: {
            show: false
          },
          hoverAnimation: false
        }, {
          value: 2.5,
          name: '',
          itemStyle: {
            color: "rgba(0,0,0,0)",
            borderWidth: 0
          },
          tooltip: {
            show: false
          },
          hoverAnimation: false
        }]
      });
    }
  })

  const imageUrl1 =
      'data:image/png;base64,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';


  const  option = {
    backgroundColor:'#000',
    color: ['rgba(51,133,255 , 99)', 'rgba(1, 179, 238 , 0)', 'rgba(22, 75, 205 , 0)', 'rgba(52, 52, 176,0)'],
    grid: {
      // top: '15%',
      bottom: '53%',
      left: "50%",
      containLabel: false
    },
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
      ]
    },
    yAxis: [{
      type: 'category',
      inverse: true,
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        interval: 0,
        inside: true,
        textStyle: {
          // color:'#ff5656',
          fontSize: 16,
        },
        show: true,// 使用函数模板，函数参数分别为刻度数值（类目），刻度的索引
        formatter: function (params) {
          // 假设此轴的 type 为 'time'。
          const _item = data.find(item=>item.value ==params)
          return `{a|${_item.name}}{b|${_item.i}}`;
        },
        rich: {
          a: {
            width: 160,
            color: 'white',
            fontSize:14,
            fontFamily:'MicrosoftYaHei',
            fontWeight:500,
            lineHeight: 19,
            textShadowColor:'rgba(55,121,220,0.6)',
            textShadowOffsetX:0,
            textShadowOffsetY:0,
            textShadowBlur:8,
          },
          b: {
            width: 64,
            align:'right',
            fontSize:20,
            lineHeight: 23,
            fontWeight:600,
            fontFamily:'pmzd',
            textShadowColor:'rgba(0,234,241,0.5)',
            textShadowOffsetX:0,
            textShadowOffsetY:0,
            textShadowBlur:12,
            height: 40
          },
          x: {
            fontSize: 18,
            fontFamily: 'Microsoft YaHei',
            borderColor: '#449933',
            borderRadius: 4
          },
        },
      },
      data: data
    }],
    xAxis: [{
      show: false
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
