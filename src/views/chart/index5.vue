<script setup>
import * as echarts from 'echarts';
const initChart = () => {
  const myChart = echarts.init(document.getElementById('main'));

  const rateColor = [ '#4BDB79' , '#0079FA' ,'#FF6600' , '#e8e8ec' ]
  const schoolColor = [ '#0964FF' , '#0079FA' ,'#0964FF' , '#0032fa' ]
  const scoreColor = [ '#0964FF' , '#C629FC' ,'#FF6600' , '#F5F6FA' ]

  var placeHolderStyle = {
    normal: {
      label: {
        show: false
      },
      labelLine: {
        show: false
      },
      color: 'rgba(0, 0, 0, 0)',
      borderColor: 'rgba(0, 0, 0, 0)',
      borderWidth: 0
    }
  };

  let rateMax = 0
  const rateSeries = [] ;
  [{
    value:100,
    name:'保'
  },{
    value:40,
    name:'稳'
  },{
    value:20,
    name:'冲'
  }].map((item,index)=>{
    rateMax = rateMax + item.value
    rateSeries.push({
      type: 'bar',
      data: [{
        name: item.name,
        value: item.value,
        itemStyle: {
          normal: {
            color: new echarts.graphic.LinearGradient(1, 0, 0, 0, [{
              offset: 0,
              color: `${rateColor[index]}`
            }, {
              offset: 0.8,
              color: `${rateColor[index]}00`
            },{
              offset: 1,
              color: `${rateColor[index]}00`
            }])
          }
        },
      }],
      stack: "rate",
      coordinateSystem: 'polar',
      polarIndex:0,
      roundCap: true,
      barWidth: 16,
      barGap: '-100%', // 两环重叠
      z: 2,
    })
  })


  rateSeries.push(
      {
        name: 'Punch Card',
        type: 'scatter',
        coordinateSystem: 'polar',
        color:'#ffffff',
        polarIndex:0,
        symbolSize: 10,
        data: [rateMax-1],
        itemStyle: {
          normal: {
            opacity:1,
            color: '#ffffff',
          }
        }
      })


  rateSeries.push({
    type: 'bar',
    data: [{
      name: '灰底',
      value: rateMax*3.6/2,
      itemStyle: {
        normal: {
          color: `${rateColor[rateColor.length-1]}70`
        },
        emphasis:{
          color: `${rateColor[rateColor.length-1]}99`
        }
      },
    }],
    coordinateSystem: 'polar',
    polarIndex:1,
    // roundCap: true,
    barWidth: 30,
    barGap: '-100%', // 两环重叠
    z: 1,
  })



  let schoolMax = 0
  const schoolSeries = [] ;
  const schoolPieData = [];
  [{
    value:100,
    name:'大专'
  },{
    value:20,
    name:'普通本科'
  },{
    value:20,
    name:'双一流'
  },{
    value:20,
    name:'清北'
  }].map((item,index)=>{
    schoolMax = schoolMax + item.value
  });


  let
      lastSchoolMax = 0;
  [{
    value:100,
    name:'大专'
  },{
    value:20,
    name:'普通本科'
  },{
    value:20,
    name:'双一流'
  },{
    value:20,
    name:'清北'
  }].map((item,index)=>{
    schoolPieData.push({
      name:item.name,
      value:item.value,

      label: {
        normal: {
          rotate:  90 - ((lastSchoolMax+ item.value/2 )/ schoolMax) *180 ,
          position: 'inner',
          formatter: '{b}',

          textStyle: {
            color: '#fff',
            fontWeight: 'bold',
            fontSize: 14
          }
        }
      },
    });
    schoolPieData.push({
      name:item.name,
      value:1,
      itemStyle:placeHolderStyle,
      label:{
        show:false
      }
    });
    lastSchoolMax = lastSchoolMax+ item.value + 1;
  });




  schoolMax = schoolMax + 3

  schoolSeries.push(
      {
        type: 'pie',
        selectedMode: 'single',
        startAngle:-180, //起始角度
        radius: [130, 170],
        color:
            schoolColor.map((item,index)=>{
              return {
                type: 'radial',
                x: myChart.getWidth()/2,
                y: myChart.getHeight()/2,
                r: 150,
                colorStops: [{
                  offset: 0, color: `${item}` // 0% 处的颜色
                }, {
                  offset: 0.2, color: `${item}` // 0% 处的颜色
                },{
                  offset: 1, color: `${item}${(index + 1)*2}0` // 100% 处的颜色
                }],
                global: true // 缺省为 false
              }
            }),
        label: {
          normal: {
            position: 'inner',
            formatter: '{b}',

            textStyle: {
              color: '#fff',
              fontWeight: 'bold',
              fontSize: 14
            }
          }
        },
        labelLine: {
          normal: {
            show: false
          }
        },
        data: [...schoolPieData , {
          value: schoolMax,
          itemStyle: placeHolderStyle,
        },]
      }
  )



  let scoreMax = 0
  const scoreSeries = [] ;
  [{
    value:600,
    name:'保'
  }].map((item,index)=>{
    scoreMax = 750
    scoreSeries.push({
      type: 'bar',
      data: [{
        name: item.name,
        value: item.value,
        itemStyle: {
          normal: {
            color: new echarts.graphic.LinearGradient(1, 1, 0, 0, [{
              offset: 0,
              color: `#1461FF`
            }, {
              offset: 0.8,
              color: `#C629FC99`
            },{
              offset: 1,
              color: `#C629FC99`
            }])
          }
        },
      }],
      stack: "score",
      coordinateSystem: 'polar',
      polarIndex:4,
      // roundCap: true,
      barWidth: 10,
      barGap: '-100%', // 两环重叠
      z: 2,
    })
    scoreSeries.push(
        {
          name: 'Punch Card',
          type: 'scatter',
          symbol: 'emptyCircle',
          coordinateSystem: 'polar',
          polarIndex:4,
          symbolSize: 20,
          data: [item],
          itemStyle: {
            normal: {
              opacity:1,
              color: '#1461FF',
              shadowColor: '#1461FF',
              shadowBlur: 1
            }
          }
        })
    // scoreSeries.push(
    //     {
    //       name: 'Punch Card',
    //       type: 'scatter',
    //       // symbol: 'emptyCircle',
    //       coordinateSystem: 'polar',
    //       color:'#ffffff',
    //       polarIndex:4,
    //       symbolSize: 14,
    //       data: [item],
    //     })
  })


  scoreSeries.push(
      {
        name: 'Punch Card',
        type: 'scatter',
        coordinateSystem: 'polar',
        color:'#ffffff',
        polarIndex:4,
        symbolSize: 20,
        data: [10],
        itemStyle: {
          normal: {
            opacity:1,
            color: '#ffffff',
            shadowColor: '#1461FF90',
            shadowBlur: 3
          }
        }
      })



  const option = {
    title: {
      text: '560分',
      textStyle: {
        color: '#01c4a3',
        fontSize: 40
      },
      subtext: '科目:物+政地 位次:5600名',
      subtextStyle: {
        color: '#909090',
      },
      itemGap: -10, // 主副标题距离
      left: 'center',
      top: 'center'
    },
    angleAxis: [
        {
      // max: 100, // 满分
      // max: 100 * 360 / 180,
      polarIndex:0,
      max:rateMax*3.6,
      clockwise: true, // 逆时针
      startAngle: -180,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      // max: 100, // 满分
      // max: 100 * 360 / 180,
      polarIndex:1,
      max:rateMax*3.6,
      clockwise: true, // 逆时针
      startAngle: -180,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      // max: 100, // 满分
      // max: 100 * 360 / 180,
      polarIndex:2,
      max:schoolMax*2,
      clockwise: true, // 逆时针
      startAngle: -180,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      // max: 100, // 满分
      // max: 100 * 360 / 180,
      polarIndex:3,
      max:schoolMax*2,
      clockwise: true, // 逆时针
      startAngle: -180,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      // max: 100, // 满分
      // max: 100 * 360 / 180,
      polarIndex:4,
      max:scoreMax*2,
      clockwise: true, // 逆时针
      startAngle: -180,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    }],
    radiusAxis: [
        {
      type: 'category',
      polarIndex:0,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      type: 'category',
      polarIndex:1,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      type: 'category',
      polarIndex:2,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      type: 'category',
      polarIndex:3,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    },{
      type: 'category',
      polarIndex:4,
      // 隐藏刻度线
      axisLine: {
        show: false
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false
      },
      splitLine: {
        show: false
      }
    }],
    polar: [
        {
      polarIndex:0,
      center: ['50%', '50%'],
      radius: '200' //图形大小
    }, {
      polarIndex:1,
      center: ['50%', '50%'],
      radius: '200' //图形大小
    }, {
      polarIndex:2,
      center: ['50%', '50%'],
      radius: '300' //图形大小
    }, {
      polarIndex:3,
      center: ['50%', '50%'],
      radius: '300' //图形大小
    }, {
      polarIndex:4,
      center: ['50%', '50%'],
      radius: '350' //图形大小
    }],
    series: [...rateSeries , ...schoolSeries, ...scoreSeries]
  }



  myChart.setOption(option);
};

nextTick(async () => {
  initChart();
});
</script>

<template>
  <div id="main" class="w-[720px] h-[480px]"></div>
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
