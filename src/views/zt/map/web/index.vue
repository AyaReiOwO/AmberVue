<script setup>
import api from '@/api';
import G6 from '@antv/g6/dist/g6.min';

import * as echarts from 'echarts';

const apiHeader = 'http://113.57.92.254:8002/api/v1';
const apiHost = 'http://113.57.92.254:8002';

const activePage = ref(0);

const pageInfo = reactive({
  current: 1,
  size: 10,
  pages: 10
});

let activeProvinceInfo = reactive({ id: 1, name: 'China' });

let activeCaseInfo = ref(null);

let caseList = reactive([]);

const provinceList = [
  {
    id: 2,
    name: '北京市',
    parentId: 1,
    lat: 40.072363,
    lng: 116.452562,
    pinYin: 'Beijing'
  },
  {
    id: 3,
    name: '天津市',
    parentId: 1,
    lat: 39.133066,
    lng: 117.262043,
    pinYin: 'Tianjin'
  },
  {
    id: 4,
    name: '河北省',
    parentId: 1,
    lat: 38.644184,
    lng: 115.643081,
    pinYin: 'Hebei'
  },
  {
    id: 5,
    name: '山西省',
    parentId: 1,
    lat: 37.857014,
    lng: 112.549248,
    pinYin: 'Shanxi'
  },
  {
    id: 6,
    name: '内蒙古自治区',
    parentId: 1,
    lat: 43.163929,
    lng: 112.846691,
    pinYin: 'NeiMengGu'
  },
  {
    id: 7,
    name: '辽宁省',
    parentId: 1,
    lat: 41.470982,
    lng: 123.075589,
    pinYin: 'Liaoning'
  },
  {
    id: 8,
    name: '吉林省',
    parentId: 1,
    lat: 43.593362,
    lng: 126.460691,
    pinYin: null
  },
  {
    id: 9,
    name: '黑龙江省',
    parentId: 1,
    lat: 47.220176,
    lng: 128.153242,
    pinYin: null
  },
  {
    id: 10,
    name: '上海市',
    parentId: 1,
    lat: 31.262235,
    lng: 121.456626,
    pinYin: null
  },
  {
    id: 11,
    name: '江苏省',
    parentId: 1,
    lat: 33.015387,
    lng: 119.837664,
    pinYin: null
  },
  {
    id: 12,
    name: '浙江省',
    parentId: 1,
    lat: 29.217657,
    lng: 120.426378,
    pinYin: null
  },
  {
    id: 13,
    name: '安徽省',
    parentId: 1,
    lat: 32.174367,
    lng: 117.041275,
    pinYin: null
  },
  {
    id: 14,
    name: '福建省',
    parentId: 1,
    lat: 26.371211,
    lng: 118.071524,
    pinYin: null
  },
  {
    id: 15,
    name: '江西省',
    parentId: 1,
    lat: 27.787491,
    lng: 115.863848,
    pinYin: null
  },
  {
    id: 16,
    name: '山东省',
    parentId: 1,
    lat: 36.119406,
    lng: 117.777167,
    pinYin: null
  },
  {
    id: 17,
    name: '河南省',
    parentId: 1,
    lat: 34.307663,
    lng: 113.87694,
    pinYin: null
  },
  {
    id: 18,
    name: '湖北省',
    parentId: 1,
    lat: 31.262235,
    lng: 112.03721,
    pinYin: null
  },
  {
    id: 19,
    name: '湖南省',
    parentId: 1,
    lat: 27.656511,
    lng: 112.03721,
    pinYin: null
  },
  {
    id: 20,
    name: '广东省',
    parentId: 1,
    lat: 23.994144,
    lng: 113.656173,
    pinYin: null
  },
  {
    id: 21,
    name: '广西壮族自治区',
    parentId: 1,
    lat: 24.129297,
    lng: 109.020054,
    pinYin: null
  },
  {
    id: 22,
    name: '海南省',
    parentId: 1,
    lat: 19.317629,
    lng: 109.903124,
    pinYin: null
  },
  {
    id: 23,
    name: '重庆市',
    parentId: 1,
    lat: 30.053442,
    lng: 107.842627,
    pinYin: null
  },
  {
    id: 24,
    name: '四川省',
    parentId: 1,
    lat: 31.135715,
    lng: 102.544205,
    pinYin: 'SiChuan'
  },
  {
    id: 25,
    name: '贵州省',
    parentId: 1,
    lat: 27.196833,
    lng: 107.106735,
    pinYin: null
  },
  {
    id: 26,
    name: '云南省',
    parentId: 1,
    lat: 24.39917,
    lng: 101.513957,
    pinYin: null
  },
  {
    id: 27,
    name: '西藏',
    parentId: 1,
    lat: 29.660361,
    lng: 91.132212,
    pinYin: null
  },
  {
    id: 28,
    name: '陕西省',
    parentId: 1,
    lat: 34.855464,
    lng: 109.020054,
    pinYin: null
  },
  {
    id: 29,
    name: '甘肃省',
    parentId: 1,
    lat: 38.122853,
    lng: 102.249849,
    pinYin: null
  },
  {
    id: 30,
    name: '青海省',
    parentId: 1,
    lat: 36.238732,
    lng: 95.038108,
    pinYin: null
  },
  {
    id: 31,
    name: '宁夏回族自治区',
    parentId: 1,
    lat: 37.421868,
    lng: 106.150075,
    pinYin: null
  },
  {
    id: 32,
    name: '新疆',
    parentId: 1,
    lat: 41.526309,
    lng: 85.324335,
    pinYin: null
  }
];

const cityList = [];

const provinceToMap = {
  China: '/asset/get/s/data-1527045631990-r1dZ0IM1X.json',
  上海市: '/asset/get/s/data-1482909900836-H1BC_1WHg.json',
  河北省: '/asset/get/s/data-1482909799572-Hkgu_yWSg.json',
  山西省: '/asset/get/s/data-1482909909703-SyCA_JbSg.json',
  内蒙古自治区: '/asset/get/s/data-1482909841923-rkqqdyZSe.json',
  辽宁省: '/asset/get/s/data-1482909836074-rJV9O1-Hg.json',
  吉林省: '/asset/get/s/data-1482909832739-rJ-cdy-Hx.json',
  黑龙江省: '/asset/get/s/data-1482909803892-Hy4__J-Sx.json',
  江苏省: '/asset/get/s/data-1482909823260-HkDtOJZBx.json',
  浙江省: '/asset/get/s/data-1482909960637-rkZMYkZBx.json',
  安徽省: '/asset/get/s/data-1482909768458-HJlU_yWBe.json',
  福建省: '/asset/get/s/data-1478782908884-B1H6yezWe.json',
  江西省: '/asset/get/s/data-1482909827542-r12YOJWHe.json',
  山东省: '/asset/get/s/data-1482909892121-BJ3auk-Se.json',
  河南省: '/asset/get/s/data-1482909807135-SJPudkWre.json',
  湖北省: '/asset/get/s/data-1482909813213-Hy6u_kbrl.json',
  湖南省: '/asset/get/s/data-1482909818685-H17FOkZSl.json',
  广东省: '/asset/get/s/data-1482909784051-BJgwuy-Sl.json',
  广西壮族自治区: '/asset/get/s/data-1482909787648-SyEPuJbSg.json',
  海南省: '/asset/get/s/data-1482909796480-H12P_J-Bg.json',
  四川省: '/asset/get/s/data-1482909931094-H17eKk-rg.json',
  贵州省: '/asset/get/s/data-1482909791334-Bkwvd1bBe.json',
  云南省: '/asset/get/s/data-1482909957601-HkA-FyWSx.json',
  西藏: '/asset/get/s/data-1482927407942-SkOV6Qbrl.json',
  陕西省: '/asset/get/s/data-1482909918961-BJw1FyZHg.json',
  甘肃省: '/asset/get/s/data-1482909780863-r1aIdyWHl.json',
  青海省: '/asset/get/s/data-1482909853618-B1IiOyZSl.json',
  宁夏回族自治区: '/asset/get/s/data-1482909848690-HJWiuy-Bg.json',
  新疆: '/asset/get/s/data-1482909952731-B1YZKkbBx.json',
  北京市: '/asset/get/s/data-1482818963027-Hko9SKJrg.json',
  天津市: '/asset/get/s/data-1482909944620-r1-WKyWHg.json',
  重庆市: '/asset/get/s/data-1482909775470-HJDIdk-Se.json',
  香港: '/asset/get/s/data-1461584707906-r1hSmtsx.json',
  澳门: '/asset/get/s/data-1482909771696-ByVIdJWBx.json'
};

const loadProvinceMapJson = async (mapName, url) => {
  //加载时的文字提示
  if (!echarts.getMap(mapName)) {
    await api.axios.get(`http://api.ppmark.cn/chart-assets/${url}`).then(geoJson => {
      echarts.registerMap(mapName, geoJson);
    });
  }
};

provinceList.map(async item => {
  console.log(item.name, provinceToMap[item.name]);
  await loadProvinceMapJson(item.name, provinceToMap[item.name]);
});

let chinaChart = null;

let provinceChart = null;

const flagList = [];

const initTree = () => {
  const container = document.getElementById('container');
  const width = container.scrollWidth || 800;
  const height = container.scrollHeight || 800;

  const registerNode = () => {
    G6.registerEdge('dice-er-edge', {
      afterDraw(cfg, group) {
        {
          const shape = group.get('children')[0];
          let index = 0;
          // Define the animation
          const lineDash = [4, 2, 1, 2];
          shape.animate(
            () => {
              index++;
              if (index > 9) {
                index = 0;
              }
              const res = {
                lineDash,
                lineDashOffset: -index
              };
              // returns the modified configurations here, lineDash and lineDashOffset here
              return res;
            },
            {
              repeat: true, // whether executes the animation repeatly
              duration: 3000 // the duration for executing once
            }
          );
        }
      }
    });
    G6.registerNode(
      'lineNode',
      {
        drawText(cfg, group) {
          group.addShape('text', {
            attrs: {
              // text:cfg.label,
              // fill:'white',
              // ...cfg.labelCfg.style
            }
          });
        },
        draw(cfg, group) {
          //1.
          const attrs = cfg;
          let pointCount = 0;

          cfg.details.forEach(detail => {
            console.log({ pointCount });
            const { color, values } = detail;
            values.forEach((value, index) => {
              pointCount++;

              {
                const itemHeight = 40;
                const yArr = [
                  itemHeight * -8,
                  itemHeight * -7,
                  itemHeight * -6,
                  itemHeight * -5,
                  itemHeight * -4,
                  itemHeight * -3,
                  itemHeight * -2,
                  -1 * itemHeight,
                  0,
                  itemHeight,
                  itemHeight * 2,
                  itemHeight * 3,
                  itemHeight * 4,
                  itemHeight * 5,
                  itemHeight * 6,
                  itemHeight * 7,
                  itemHeight * -7,
                  itemHeight * -6,
                  itemHeight * -5,
                  itemHeight * -4,
                  itemHeight * -3,
                  itemHeight * -2,
                  -1 * itemHeight,
                  0,
                  itemHeight,
                  itemHeight * 2,
                  itemHeight * 3,
                  itemHeight * 4,
                  itemHeight * 5,
                  itemHeight * 6,
                  itemHeight * 7
                  // itemHeight*7,itemHeight*6,itemHeight*5,itemHeight*4,itemHeight*3,itemHeight*2,36,0,-36 ,itemHeight*(-2) ,itemHeight*(-3) ,itemHeight*(-4) ,itemHeight*(-5) ,itemHeight*(-6) ,itemHeight*(-7) ,
                ];
                const rMin = 760,
                  rMax = rMin;
                const rArr = [
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin
                ];
                const boxHeight = 28;
                const y = yArr[pointCount % yArr.length];
                const r = rArr[pointCount % rArr.length];
                let x = pointCount % 2 ? Math.sqrt(r * r - y * y) : -1 * Math.sqrt(r * r - y * y);
                // x = x > 0 ? x : x - 14;
                group.addShape('image', {
                  attrs: {
                    width: 24,
                    height: 24,
                    x: x > 0 ? x - 24 : x,
                    y: y + 21 - boxHeight / 2,
                    img: 'data:image/png;base64,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',
                    cursor: 'pointer'
                  },
                  draggable: true
                });

                group.addShape('text', {
                  attrs: {
                    text: `${value.name}`,
                    fill: '#ffffff',
                    height: itemHeight,
                    x: x,
                    y: y + 20,
                    textAlign: x > 0 ? 'start' : 'end',
                    textBaseline: 'middle',
                    fontSize: 12,
                    fontWeight: 500
                  },
                  name: ''
                });
              }
            });
          });

          // group.addShape('text', {
          //   attrs: {
          //     text: '云启智慧全国业务分布',
          //     fill: 'white',
          //     width: 400,
          //     // height: 40,
          //     x: 0,
          //     y: height / -2 + 150,
          //     textAlign: 'center',
          //     textBaseline: 'middle',
          //     fontSize: 25,
          //     fontWeight: 900
          //   }
          // });

          const shape = group;
          this.drawText(cfg, group);
          return shape;
        }
      },
      'extendNodeName'
    );
  };

  registerNode();
  const data = {
    nodes: [
      {
        id: 'node1',
        label: 'Node 1',
        x: width / 2,
        y: height / 2,
        centerColor: '#bae7ff',
        details: [
          {
            cat: 'pv',
            values: provinceList,
            color: '#5B8FF9'
          }
        ]
      }
      // {
      //   id:'node2',
      //   label:'Node 2',
      //   x:400,
      //   y:150,
      //   centerColor:"#bae7ff",
      //   details:[
      //     {cat:'pv',values :[
      //         10086,30,40,30,30,20,30,40,30,30,
      //         20,30,40,30,30,20,30,40,30,30,
      //         20,30,40,30,30,20,30,40,30,30,
      //       ] , color:'#5B8FF9'},
      //   ]
      // }
    ],
    edges: [
      {
        source: 'node1',
        sourceKey: 'source-10086',
        target: 'node2',
        targetKey: 'target-10086'
      }
    ]
  };
  const graph = new G6.Graph({
    // 控件
    container: container,
    // 宽度
    width: width,
    // 高度
    height: height,
    // 布局
    layout: {
      type: 'dagre'
    },
    // 节点
    defaultNode: {
      type: 'lineNode',
      style: {
        width: 180,
        height: 60,
        radius: 10
      },
      labelCfg: {
        style: {
          fontSize: 20
        }
      },
      anchorPoints: [
        [0, 0],
        [1, 0],
        [0, 1],
        [1, 1]
      ]
    },
    fitCenter: true,
    // 边
    defaultEdge: {
      type: 'dice-er-edge',
      style: {
        stroke: '#e11d1d',
        lineWidth: 4,
        endArrow: false
      }
    },
    // 适应视图的大小
    fitView: true,
    // 添加渲染视图 padding
    fitViewPadding: true,
    modes: {
      default: []
    },
    nodeStateStyles: {
      hover: {
        fill: 'lightsteelblue'
      },
      click: {
        stroke: 'steelblue',
        lineWidth: 10
      }
    }
  });

  graph.read(data);

  graph.on('canvas:click', event => {
    console.log(event);
  });

  graph.on('node:click', event => {
    console.log(event);
    graph.setItemState(event.item, 'click', true);
  });

  graph.on('node:mouseenter', event => {
    console.log(event);
  });

  graph.on('item-content:mouseenter', e => {
    const { target } = e;
    target.animate(
      {
        shadowColor: 'rgba(44,104,255,0.3500)',
        shadowBlur: 10
      },
      {
        duration: 0,
        repeat: false
      }
    );
  });

  graph.on('item-content:mouseleave', e => {
    const { target } = e;
    target.animate(
      {
        shadowColor: 'rgba(44,104,255,0.0500)',
        shadowBlur: 10
      },
      {
        duration: 0,
        repeat: false
      }
    );
  });
};

const initProvinceChart = async () => {
  const mapName = activeProvinceInfo.name;

  /*
  喜欢作品的可以关注csdn https://blog.csdn.net/qq_47410017
  */

  if (!provinceChart) {
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  } else {
    provinceChart.dispose();
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  }

  //调用中国地图（同步）

  const dataTemp = flagList.map(item => {
    return {
      name: item.name,
      lng: item.lng,
      lat: item.lat
    };
  });

  const effectScatterList = cityList
    .filter(cityItem => !flagList.find(flagItem => flagItem.name === cityItem.name))
    .map(item => {
      return {
        name: item.name,
        value: [item.lng, item.lat, null]
      };
    });

  const option = {
    grid: {
      height: '100%',
      width: '100%'
    },
    geo: [
      {
        map: mapName,
        label: {
          normal: {
            show: false
          },
          emphasis: {
            show: false
          }
        },
        top: 'center',
        left: 'center',
        roam: false,
        zoom: 1.01,

        itemStyle: {
          normal: {
            areaColor: '#0066FF00',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          },
          emphasis: {
            areaColor: '#0066FF',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          }
        }
      }
    ],

    series: [
      {
        type: 'map',
        mapType: mapName,
        top: 'center',
        left: 'center',
        label: {
          normal: {
            show: false,
            position: 'inner',
            textStyle: {
              color: '#fff',
              fontSize: 14,
              fontWeight: 'bold',
              align: 'left'
            }
          },
          emphasis: {
            show: false,
            textStyle: {
              color: '#fff'
            }
          }
        },
        select: {
          itemStyle: {
            areaColor: '#0066FF',
            borderColor: '#fff',
            borderWidth: 2
          },
          label: {
            show: false
          }
        },
        itemStyle: {
          normal: {
            areaColor: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#0066FF70' // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: '#006690' // 100% 处的颜色
                }
              ],
              global: false // 缺省为 false
            },

            borderColor: '#ffffff30',
            borderWidth: 1,
            shadowColor: '#ffffff50',
            shadowBlur: 10
          },
          emphasis: {
            areaColor: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#0066FF80' // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: '#006699' // 100% 处的颜色
                }
              ],
              global: false // 缺省为 false
            },
            borderColor: '#fff',
            borderWidth: 3
          }
        }
      },

      {
        type: 'effectScatter',
        coordinateSystem: 'geo',
        data: effectScatterList,
        symbolSize: function (val) {
          return 12;
        },
        showEffectOn: 'render',
        rippleEffect: {
          brushType: 'stroke'
        },
        hoverAnimation: true,
        label: {
          normal: {
            formatter: '  {b}',
            position: 'right',
            show: true,
            color: '#fff',
            fontSize: 14,
            align: 'left',
            margin: 10
          }
        },
        itemStyle: {
          normal: {
            color: '#fff',
            shadowBlur: 10,
            shadowColor: '#fff'
          }
        }
      },

      {
        type: 'custom',
        coordinateSystem: 'geo',
        renderItem: function (params, api) {
          //自定义图标
          const item = dataTemp[params.dataIndex];
          return {
            type: 'group',
            children: [
              {
                type: 'rect',
                name: item.name,
                style: {
                  fill: '#00000000',
                  stroke: '#00000000',
                  lineWidth: 0
                },
                shape: {
                  x: api.coord([item.lng, item.lat])[0] - 54,
                  y: api.coord([item.lng, item.lat])[1] - 88,
                  width: 80,
                  height: 80,
                  r: 5
                },
                textContent: {
                  style: {
                    text: item.name,
                    color: '#fff',
                    fontSize: 14,
                    fontWeight: 'bold',
                    align: 'left'
                  }
                },
                textConfig: {
                  position: 'inside'
                }
              },
              {
                type: 'image',
                name: item.name,
                style: {
                  image: '/src/assets/images/zt/web/quanguo/red-flag.png',
                  x: api.coord([item.lng, item.lat])[0] - 24,
                  y: api.coord([item.lng, item.lat])[1] - 40,
                  width: 48,
                  height: 48
                }
              },

              {
                type: 'image',
                name: item.name,
                style: {
                  image: '/src/assets/images/zt/web/quanguo/province-name-bg.png',
                  x: api.coord([item.lng, item.lat])[0] - 20,
                  y: api.coord([item.lng, item.lat])[1] - 60,
                  width: 54,
                  height: 24
                }
              }
            ]
          };
        },
        label: {
          normal: {
            show: false,
            position: 'inside',
            color: '#000'
          }
        },
        z: 90,
        data: dataTemp
      }
    ]
  };

  provinceChart.setOption(option);

  //点击事件
  provinceChart.on('click', function (e) {
    //高亮鼠标移入的位置
    provinceChart.dispatchAction({
      type: 'select ',
      seriesIndex: 0,
      dataIndex: e.dataIndex
    });
  });
};

const loadProvinceData = async () => {
  const result = await api.axios.get(
    `/api/v1/screen/business/list-by-province-id?current=${pageInfo.current}&pageSize=${pageInfo.size}&provinceId=${activeProvinceInfo.id}`
  );
  caseList.length = 0;
  caseList.push(...result.data.records);
  pageInfo.pages = result.data.pages;
};

const loadCityList = async () => {
  const result = await api.axios.get(`/api/v1/screen/regions/city/list?provinceId=${activeProvinceInfo.id}`);
  cityList.length = 0;
  cityList.push(...result.data);
};

const loadFlagData = async () => {
  const result = await api.axios.get(`/api/v1/screen/business/city/flag?provinceId=${activeProvinceInfo.id}`);
  flagList.length = 0;
  flagList.push(...result.data);
};

const initProvince = async () => {
  if (!provinceChart) {
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  } else {
    provinceChart.dispose();
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  }
  caseList.length = 0;

  const mapName = activeProvinceInfo.name;
  await Promise.all([loadProvinceMapJson(mapName, provinceToMap[mapName]), loadFlagData(), loadCityList(), loadProvinceData()]);
  initProvinceChart();
};

const initChinaChart = async () => {
  /*
  喜欢作品的可以关注csdn https://blog.csdn.net/qq_47410017
  */

  if (!chinaChart) {
    chinaChart = echarts.init(document.getElementById('main'));
  }

  const mapName = 'China';

  //加载时的文字提示
  chinaChart.showLoading({ text: '正在加载数据' }); //增加等待提示
  await loadProvinceMapJson(mapName, provinceToMap[mapName]);
  chinaChart.hideLoading();

  const dataTemp = provinceList.map(item => {
    return {
      name: item.name,
      lng: item.lng,
      lat: item.lat
    };
  });

  var option = {
    // backgroundColor: '#00094A',
    // tooltip: {
    //   trigger: 'item',
    //   axisPointer: {
    //     type: 'shadow'
    //   }
    // },

    grid: {
      height: '100%',
      width: '100%'
    },
    geo: [
      {
        map: 'China',
        label: {
          normal: {
            show: false
          },
          emphasis: {
            show: false
          }
        },
        top: 'center',
        left: 'center',
        roam: false,
        zoom: 1.01,

        itemStyle: {
          normal: {
            areaColor: '#0066FF00',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          },
          emphasis: {
            areaColor: '#0066FF',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          }
        }
      }
    ],

    series: [
      {
        type: 'map',
        mapType: 'China',
        top: 'center',
        left: 'center',
        label: {
          normal: {
            show: false,
            position: 'inner',
            textStyle: {}
          },
          emphasis: {
            show: false
          }
        },
        select: {
          itemStyle: {
            areaColor: '#0066FF',
            borderColor: '#fff',
            borderWidth: 2
          },
          label: {
            show: false
          }
        },
        itemStyle: {
          normal: {
            areaColor: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#0066FF70' // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: '#006690' // 100% 处的颜色
                }
              ],
              global: false // 缺省为 false
            },

            borderColor: '#ffffff30',
            borderWidth: 1,
            shadowColor: '#ffffff50',
            shadowBlur: 10
          },
          emphasis: {
            areaColor: '#0066FF',
            borderColor: '#fff',
            borderWidth: 3
          }
        }
      },
      {
        type: 'custom',
        coordinateSystem: 'geo',
        renderItem: function (params, api) {
          //自定义图标
          const item = dataTemp[params.dataIndex];
          return {
            type: 'group',
            children: [
              {
                type: 'rect',
                name: item.name,
                style: {
                  fill: '#00000000',
                  stroke: '#00000000',
                  lineWidth: 0
                },
                shape: {
                  x: api.coord([item.lng, item.lat])[0] - 54,
                  y: api.coord([item.lng, item.lat])[1] - 88,
                  width: 80,
                  height: 80,
                  r: 5
                },
                textContent: {
                  style: {
                    text: item.name,
                    color: '#fff',
                    fontSize: 14,
                    fontWeight: 'bold',
                    align: 'left'
                  }
                },
                textConfig: {
                  position: 'inside'
                }
              },
              {
                type: 'image',
                name: item.name,
                style: {
                  image: '/src/assets/images/zt/web/quanguo/red-flag.png',
                  x: api.coord([item.lng, item.lat])[0] - 24,
                  y: api.coord([item.lng, item.lat])[1] - 40,
                  width: 48,
                  height: 48
                }
              },

              {
                type: 'image',
                name: item.name,
                style: {
                  image: '/src/assets/images/zt/web/quanguo/province-name-bg.png',
                  x: api.coord([item.lng, item.lat])[0] - 20,
                  y: api.coord([item.lng, item.lat])[1] - 60,
                  width: 54,
                  height: 24
                }
              }
            ]
          };
        },
        label: {
          normal: {
            show: false,
            position: 'inside',
            color: '#000'
          }
        },
        z: 90,
        data: dataTemp
      }
    ]
  };

  chinaChart.setOption(option);

  //点击事件
  chinaChart.on('click', function (e) {
    //高亮鼠标移入的位置
    chinaChart.dispatchAction({
      type: 'select ',
      seriesIndex: 0,
      dataIndex: e.dataIndex
    });

    const chooseInfo = provinceList.find(item => {
      //我们根据名字来判断是否选择一种
      return item.name == e.name;
    });
    if (chooseInfo) {
      activeProvinceInfo = chooseInfo;
      activePage.value = 1;
    }
  });
};

const initChina = async () => {
  await initChinaChart();
};

watch(
  activePage,
  (newvalue, oldvalue) => {
    if (newvalue === 0) {
      //
    } else if (newvalue === 1 && oldvalue === 0) {
      initProvince();
    } else if (newvalue === 1 && oldvalue === 2) {
      //
    } else if (newvalue === 2) {
      //
    }
  },
  { immediate: false }
);

// watch(
//   pageInfo,
//   (newvalue, oldvalue) => {
//     console.log(newvalue.current, oldvalue.current);
//     if (newvalue.current !== oldvalue.current) {
//       loadProvinceData();
//     }
//   },
//   {
//     deep: true,
//     immediate: false
//   }
// );

nextTick(async () => {
  initTree();
  initChina();
});
</script>

<template>
  <div v-show="activePage === 0" id="quanguo" class="min-w-full min-h-full w-full h-full">
    <div id="main" class="min-w-full min-h-full w-full h-full z-10"></div>
    <div id="container" class="min-h-full h-full"></div>
    <div class="center-circle-bg"></div>
    <div class="head">云启智慧全国业务分布</div>
    <div class="footer"></div>
  </div>
  <div v-show="activePage === 1" v-if="activeProvinceInfo" id="shengfen" class="min-w-full min-h-full w-full h-full">
    <div id="provinceChart" class="min-w-full min-h-full w-full h-full z-10"></div>
    <div id="case-list" class="z-10">
      <div class="case-title mb-8">
        <div class="case-title-bg"></div>
        <div class="zhongwen">{{ activeProvinceInfo.name }}</div>
        <div class="pinyin">{{ activeProvinceInfo.pinYin }}</div>
      </div>
      <div class="case-divider"></div>
      <div
        v-for="item in caseList"
        :key="item"
        class="flex items-center case-item mt-8px"
        @click="
          activeCaseInfo = item;
          activePage = 2;
        "
      >
        <div class="ml-35px">{{ item.projectName }}</div>
      </div>
      <div v-if="caseList && caseList.length" class="case-page flex items-center mx-auto mt-4 justify-around">
        <div
          class="pre"
          @click="
            pageInfo.current = pageInfo.current - 1 > 1 ? pageInfo.current - 1 : 1;
            loadProvinceData();
          "
        ></div>
        <div>{{ pageInfo.current }}/{{ pageInfo.pages }}</div>
        <div
          class="next"
          @click="
            pageInfo.current = pageInfo.current + 1 > pageInfo.pages ? pageInfo.pages : pageInfo.current + 1;
            loadProvinceData();
          "
        ></div>
      </div>
    </div>
    <div class="center-circle-bg"></div>
    <div class="head">云启智慧全国业务分布</div>
    <div class="footer"></div>
    <div class="back-btn z-99" @click="activePage = 0"></div>
  </div>
  <div v-show="activePage === 2" v-if="activeCaseInfo" id="intro" class="min-w-full min-h-full w-full h-full flex flex-col items-center">
    <div class="head">云启智慧全国业务分布</div>
    <div class="title-bg flex justify-between mt-29 items-center">
      <div class="title ml-8">{{ activeCaseInfo.projectName }}</div>
    </div>
    <div class="content-box flex justify-between">
      <div class="center-circle-bg"></div>
      <div class="w-4/7 z-10 flex flex-col h-full">
        <div class="detailed-intro mt-6 ml-20"></div>
        <div class="product-intro mt-18">
          <div class="ml-24 title-bg"></div>
          <div class="content ml-32 mt-8">
            {{ activeCaseInfo.introduction }}
          </div>
        </div>
        <div v-if="activeCaseInfo.advantages" class="product-advantage mt-24">
          <div class="ml-24 title-bg"></div>
          <div class="advantages-item ml-40 mt-8 flex" v-for="(item, index) in activeCaseInfo.advantages.split(';;')" :key="index">
            <div></div>
            <span class="ml-4">{{ item }}</span>
          </div>
        </div>
      </div>
      <div v-if="activeCaseInfo.imageUrl" class="w-3/7 z-10 flex min-h-full h-full">
        <div class="product-img mt-7"></div>
        <div class="flex flex-col justify-evenly ml-3 py-3">
          <img v-for="(item, index) in activeCaseInfo.imageUrl.split(';;')" :key="index" class="intro-img" :src="`${apiHost}${item}`" />
        </div>
      </div>
    </div>
    <div class="footer"></div>
    <div class="back-btn z-99" @click="activePage = 1"></div>
  </div>
</template>

<style lang="scss">
#quanguo {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/images/zt/web/quanguo/quanguo.png');
  background-size: 100% 100%;

  .center-circle-bg {
    margin-top: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 1000px;
    height: 1000px;
    background-image: url('@/assets/images/zt/web/center-circle-bg.png');
    background-size: 100% 100%;
    z-index: 1;
  }

  .head {
    width: 460px;
    height: 48px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    top: 30px;
    transform: translate(-50%, -50%);
  }

  .footer {
    width: 1920px;
    height: 56px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    background-image: url('@/assets/images/zt/web/footer-bg.png');
    background-size: 100% 100%;
  }

  #main {
    text-align: center;
    border-radius: 20px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    // background-image: url('@/assets/images/zt/bg.png');
    // background-size: 100% 100%;
  }

  #container {
    margin-left: 0px;
    text-align: center;
    border-radius: 20px;
    width: 92%;
    height: 350px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}

#shengfen {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/images/zt/web/province/province.png');
  background-size: 100% 100%;

  .center-circle-bg {
    position: absolute;
    left: 42%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 1000px;
    height: 1000px;
    background-image: url('@/assets/images/zt/web/center-circle-bg.png');
    background-size: 100% 100%;
    z-index: 1;
  }

  .head {
    width: 460px;
    height: 48px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    top: 30px;
    transform: translate(-50%, -50%);
  }

  .footer {
    width: 1920px;
    height: 56px;

    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    background-image: url('@/assets/images/zt/web/footer-bg.png');
    background-size: 100% 100%;
  }

  .back-btn {
    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    width: 116px;
    height: 36px;
    background-image: url('@/assets/images/zt/web/back-btn.png');
    background-size: 100% 100%;
  }

  #provinceChart {
    text-align: center;
    border-radius: 20px;
    position: absolute;
    left: 42%;
    top: 50%;
    transform: translate(-50%, -50%);
    // background-image: url('@/assets/images/zt/bg.png');
    // background-size: 100% 100%;
  }

  #case-list {
    text-align: left;
    width: 262px;
    height: 350px;
    position: absolute;
    right: -3%;
    top: 30%;
    transform: translate(-50%, -50%);

    .case-title {
      width: 229px;
      height: 67px;
      .case-title-bg {
        margin-top: 24px;

        position: absolute;
        width: 229px;
        height: 67px;
        background-image: url('@/assets/images/zt/web/intro/case-title-bg.png');
        background-size: 100% 100%;
      }
      .zhongwen {
        margin-top: -10px;
        margin-left: 36px;

        position: absolute;
        height: 63px;
        font-size: 48px;
        font-family: jiangxizhuokai-Regular, jiangxizhuokai;
        font-weight: 400;
        color: #ffffff;
        line-height: 63px;
        letter-spacing: 2px;
      }
      .pinyin {
        margin-top: 55px;
        margin-left: 150px;

        position: absolute;
        height: 22px;
        font-size: 16px;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #ffc97a;
        line-height: 22px;
      }
    }

    .case-divider {
      width: 248px;
      height: 7px;
      background-image: url('@/assets/images/zt/web/province/case-divider.png');
      background-size: 100% 100%;
    }

    .case-item {
      width: 262px;
      height: 60px;
      background-image: url('@/assets/images/zt/web/province/case-item-bg.png');
      background-size: 100% 100%;
      div {
        width: 220px;
        height: 44px;
        font-size: 14px;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #ffffff;
        line-height: 22px;
      }
    }

    .case-page {
      width: 160px;
      height: 22px;
      font-size: 14px;
      font-family: PingFangSC-Medium, PingFang SC;
      font-weight: 500;
      color: #ffffff;
      line-height: 22px;
      .pre {
        width: 34px;
        height: 34px;
        background-image: url('@/assets/images/zt/web/province/page-pre.png');
        background-size: 100% 100%;
      }
      .next {
        width: 34px;
        height: 34px;
        background-image: url('@/assets/images/zt/web/province/page-next.png');
        background-size: 100% 100%;
      }
    }
  }
}

#intro {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/images/zt/web/intro/intro.png');
  background-size: 100% 100%;
  color: white;

  .title-bg {
    width: 1840px;
    height: 62px;
    background-image: url('@/assets/images/zt/web/intro/title.png');
    background-size: 100% 100%;
  }

  .head {
    width: 460px;
    height: 48px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    top: 30px;
    transform: translate(-50%, -50%);
  }

  .title {
    width: 704px;
    height: 45px;
    font-size: 32px;
    font-family: PingFangSC-Semibold, PingFang SC;
    font-weight: 600;
    color: #f0faff;
    line-height: 24px;
    text-shadow: 0px 0px 12px rgba(51, 133, 255, 0.7);
  }

  .content-box {
    width: 1840px;
    height: 820px;
    background-image: url('@/assets/images/zt/web/intro/content-bg.png');
    background-size: 100% 100%;
    .center-circle-bg {
      margin-top: 5px;
      position: absolute;
      left: 50%;
      top: 54%;
      transform: translate(-50%, -50%);
      width: 880px;
      height: 880px;
      background-image: url('@/assets/images/zt/web/center-circle-bg.png');
      background-size: 100% 100%;
      z-index: 1;
    }
  }

  .detailed-intro {
    width: 456px;
    height: 90px;
    background-image: url('@/assets/images/zt/web/intro/detailed-intro.png');
    background-size: 100% 100%;
  }

  .product-intro {
    .title-bg {
      width: 620px;
      height: 55px;
      background-image: url('@/assets/images/zt/web/intro/product-intro.png');
      background-size: 100% 100%;
    }
    .content {
      width: 760px;
      height: 104px;
      font-size: 18px;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 400;
      color: #cce0ff;
      line-height: 26px;
    }
  }

  .product-advantage {
    .title-bg {
      width: 620px;
      height: 55px;
      background-image: url('@/assets/images/zt/web/intro/product-advantage.png');
      background-size: 100% 100%;
    }
    .advantages-item {
      width: 740px;
      height: 26px;
      font-size: 18px;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 400;
      color: #cce0ff;
      line-height: 26px;
      div {
        width: 26px;
        height: 26px;
        background-image: url('@/assets/images/zt/web/intro/advantages-item-bg.png');
        background-size: 100% 100%;
      }
    }
  }

  .product-img {
    width: 47px;
    height: 263px;
    background-image: url('@/assets/images/zt/web/intro/product-img.png');
    background-size: 100% 100%;
  }

  .intro-img {
    width: 654px;
    height: 368px;
    border: 2px solid #3385ff;
  }

  .footer {
    width: 1920px;
    height: 56px;

    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    background-image: url('@/assets/images/zt/web/footer-bg.png');
    background-size: 100% 100%;
  }

  .back-btn {
    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    width: 116px;
    height: 36px;
    background-image: url('@/assets/images/zt/web/back-btn.png');
    background-size: 100% 100%;
  }
}
</style>

<route lang="yml">
meta:
  layout: blank
</route>
