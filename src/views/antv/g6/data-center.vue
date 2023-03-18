<template>

  <div id="container" ></div>

</template>

<script setup>

import G6 from '@antv/g6/dist/g6.min'
const { getLabelPosition, transform } = G6.Util;

let graph = null

const fittingString = (str, maxWidth, fontSize)=> {
  const ellipsis = '...'
  const ellipsisLength = G6.Util.getTextSize(ellipsis, fontSize)[0]
  let currentWidth = 0
  let res = str
  const pattern = new RegExp('[\u4E00-\u9FA5]+') // distinguish the Chinese charactors and letters
  str.split('').forEach((letter, i) => {
    if (currentWidth > maxWidth - ellipsisLength) return
    if (pattern.test(letter)) {
      // Chinese charactors
      currentWidth += fontSize
    } else {
      // get the width of single letter according to the fontSize
      currentWidth += G6.Util.getLetterWidth(letter, fontSize)
    }
    if (currentWidth > maxWidth - ellipsisLength) {
      res = `${str.substr(0, i)}${ellipsis}`
    }
  })
  return res
}

const graphData = {
  "nodes": [ {
    "id": "2466",
    "mainColor": "#13CC77",
    "icon": null,
    "boxType": -1,
    "table": "原始库1",
    "label": "ods_yunkt_zy_blade_resource",
    "attrs": [],
    "comboId": 'combo1'
  },{
    "id": "2460",
    "mainColor": "#13CC77",
    "icon": null,
    "boxType": -1,
    "table": "原始库",
    "label": "ods_yunkt_zy_blade_resource",
    "attrs": [],
    "comboId": 'combo1'
  }, {
    "id": "2468",
    "mainColor": "#13CC77",
    "icon": null,
    "boxType": -1,
    "table": "原始库1",
    "label": "ods_yunkt_zy_blade_resource",
    "attrs": [],
    "comboId": 'combo1'
  }, {
    "id": "13312",
    "mainColor": "#2C68FF",
    "icon": null,
    "shape": 'dice-er-center',
    "boxType": 0,
    "table": "标准库",
    "label": "dwd_yunkt_blade_resource",
    "attrs": []
  }, {
    "id": "671",
    "mainColor": "#d9bc99",
    "icon": null,
    "boxType": -1,
    "table": "原始库",
    "label": "ods_yunkt_jc_blade_user",
    "attrs": []
  },  {
    "id": "672",
    "mainColor": "#d9bc99",
    "icon": null,
    "boxType": -1,
    "table": "原始库",
    "label": "ods_yunkt_jc_blade_user",
    "attrs": []
  }, {
    "id": "6726",
    "mainColor": "#d9bc99",
    "icon": null,
    "boxType": -1,
    "table": "原始库",
    "label": "ods_yunkt_jc_blade_user",
    "attrs": []
  }, {
    "id": "13453",
    "mainColor": "#FF7D00",
    "icon": null,
    "boxType": 1,
    "table": "主题库",
    "label": "dwm_gxjw_yunktzyxxb",
    "attrs": []
  }, {
    "id": "1607197760534634499",
    "mainColor": "#FF7D00",
    "icon": null,
    "boxType": 1,
    "table": "数据中台系统",
    "label": "云课堂资源信息",
    "attrs": []
  }],
  combos: [
    // { id: 'combo1', label: 'Combo 1',  },
  ],
  "edges": [{
    "source": "2460",
    "target": "13312",
  }, {
    "source": "672",
    "target": "13312",
  }, {
    "source": "2461",
    "target": "2460",
  }, {
    "source": "673",
    "target": "672",
  }, {
    "source": "13312",
    "target": "13453",
  }, {
    "source": "13312",
    "target": "1607197760534634499",
  }]
}
const  initGraph = (data)=> {
  const { Util, registerEdge, registerNode } = G6

  registerEdge('dice-er-edge', {
    draw(cfg, group) {
      const edge = group.cfg.item
      const sourceNode = edge.getSource().getModel()
      const targetNode = edge.getTarget().getModel()

      let sourceY = 20

      if (Number(sourceNode.boxType) === 0) {
        sourceY += 12
      }

      let targetY = 20

      if (Number(targetNode.boxType) === 0) {
        targetY += 12
      }

      const startPoint = {
        ...cfg.startPoint
      }
      const endPoint = {
        ...cfg.endPoint
      }

      startPoint.y = startPoint.y + sourceY
      endPoint.y = endPoint.y + targetY

      let lineType = sourceNode.boxType || targetNode.boxType ;

      let shape
      if (sourceNode.id !== targetNode.id) {
        shape = group.addShape('path', {
          attrs: {
            path: [
              ['M', startPoint.x, startPoint.y],
              [
                'C',
                endPoint.x / 3 + (2 / 3) * startPoint.x,
                startPoint.y,
                endPoint.x / 3 + (2 / 3) * startPoint.x,
                endPoint.y,
                endPoint.x,
                endPoint.y
              ]
            ],
            stroke: lineType > 0 ? `l(0) 0:${targetNode.mainColor}10    1:${targetNode.mainColor}99 `:`l(0) 0:${sourceNode.mainColor}99  1:${sourceNode.mainColor}10 `,
            lineWidth: 16,
            endArrow: false
          },
          type: 'circle-running',
          name: 'path-shape'
        })
      }

      return shape
    },
    afterDraw(cfg, group) {
      const labelCfg = cfg.labelCfg || {}
      const edge = group.cfg.item
      const sourceNode = edge.getSource().getModel()
      const targetNode = edge.getTarget().getModel()
      if (sourceNode.collapsed && targetNode.collapsed) {
        return
      }
      const path = group.find(element => element.get('name') === 'path-shape')

      const labelStyle = Util.getLabelPosition(path, 0.5, 0, 0, true)
      const label = group.addShape('text', {
        attrs: {
          ...labelStyle,
          text: cfg.label || '',
          fill: '#000',
          textAlign: 'center',
          stroke: '#fff',
          lineWidth: 1
        }
      })
      label.rotateAtStart(labelStyle.rotate)

      // get the first shape in the group, it is the edge's path here=
      {


        let lineType = sourceNode.boxType || targetNode.boxType ;
        // get the first shape in the group, it is the edge's path here=
        const shape = group.get("children")[0];

        const arrow = group.addShape("marker", {
          attrs: {
            x: 16,
            y: 0,
            r: 8,
            lineWidth: 2,
            stroke: lineType > 0 ? `${targetNode.mainColor}70`:`${sourceNode.mainColor}70`,
            fill: "#ffffff00",
            symbol: (x, y, r) => {
              return [
                ["M", x - 6, y - 8],
                ["L", x +2, y],
                ["L", x - 6, y + 8],


              ];
            }
          }
        });

        const arrow2 = group.addShape("marker", {
          attrs: {
            x: 16,
            y: 0,
            r: 8,
            lineWidth: 2,
            stroke: lineType > 0 ? `${targetNode.mainColor}50`:`${sourceNode.mainColor}50`,
            fill: "#ffffff00",
            symbol: (x, y, r) => {
              return [
                ["M", x - 6, y - 8],
                ["L", x +2, y],
                ["L", x - 6, y + 8],

              ];
            }
          }
        });

        const arrow3 = group.addShape("marker", {
          attrs: {
            x: 16,
            y: 0,
            r: 8,
            lineWidth: 2,
            stroke: lineType > 0 ? `${targetNode.mainColor}30`:`${sourceNode.mainColor}30`,
            fill: "#ffffff00",
            symbol: (x, y, r) => {
              return [
                ["M", x - 6, y - 8],
                ["L", x +2, y],
                ["L", x - 6, y + 8],

              ];
            }
          }
        });

        setTimeout(()=>{
          arrow2.animate(
              (ratio) => {
                // the operations in each frame. Ratio ranges from 0 to 1 indicating the prograss of the animation. Returns the modified configurations
                // get the position on the edge according to the ratio
                const tmpPoint = shape.getPoint(ratio);
                const pos = getLabelPosition(shape, ratio);
                let matrix = [1, 0, 0, 0, 1, 0, 0, 0, 1];
                matrix = transform(matrix, [
                  ["t", -tmpPoint.x, -tmpPoint.y],
                  ["r", pos.angle],
                  ["t", tmpPoint.x, tmpPoint.y]
                ]);

                // returns the modified configurations here, x and y here
                return {
                  x: tmpPoint.x,
                  y: tmpPoint.y,
                  matrix
                };
              },
              {
                repeat: true, // Whether executes the animation repeatly
                duration: 3000 // the duration for executing once
              }
          );
        },500)

        setTimeout(()=>{
          arrow3.animate(
              (ratio) => {
                // the operations in each frame. Ratio ranges from 0 to 1 indicating the prograss of the animation. Returns the modified configurations
                // get the position on the edge according to the ratio
                const tmpPoint = shape.getPoint(ratio);
                const pos = getLabelPosition(shape, ratio);
                let matrix = [1, 0, 0, 0, 1, 0, 0, 0, 1];
                matrix = transform(matrix, [
                  ["t", -tmpPoint.x, -tmpPoint.y],
                  ["r", pos.angle],
                  ["t", tmpPoint.x, tmpPoint.y]
                ]);

                // returns the modified configurations here, x and y here
                return {
                  x: tmpPoint.x,
                  y: tmpPoint.y,
                  matrix
                };
              },
              {
                repeat: true, // Whether executes the animation repeatly
                duration: 3000 // the duration for executing once
              }
          );
        },1000)

        arrow.animate(
            (ratio) => {
              // the operations in each frame. Ratio ranges from 0 to 1 indicating the prograss of the animation. Returns the modified configurations
              // get the position on the edge according to the ratio
              const tmpPoint = shape.getPoint(ratio);
              const pos = getLabelPosition(shape, ratio);
              let matrix = [1, 0, 0, 0, 1, 0, 0, 0, 1];
              matrix = transform(matrix, [
                ["t", -tmpPoint.x, -tmpPoint.y],
                ["r", pos.angle],
                ["t", tmpPoint.x, tmpPoint.y]
              ]);

              // returns the modified configurations here, x and y here
              return {
                x: tmpPoint.x,
                y: tmpPoint.y,
                matrix
              };
            },
            {
              repeat: true, // Whether executes the animation repeatly
              duration: 3000 // the duration for executing once
            }
        );
      }
    }
  })

  registerNode('dice-er-box', {
    draw(cfg, group) {
      let { collapsed = true, mainColor, boxType, icon } = cfg

      let width = 215,
          height = 200
      const boxStyle = {
        stroke: mainColor,
        radius: 2
      }

      if (collapsed === false) {
        height = 200
      } else {
        height = 60
      }


      // 标题右侧图片
      group.addShape('image', {
        attrs: {
          height: 40,
          width,
          img: 'data:image/png;base64,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'
        },
        draggable: true
      })


      // 标题文字
      group.addShape('text', {
        attrs: {
          y: 30,
          x: width/2,
          textAlign: 'center',
          fill: mainColor,
          text: fittingString(cfg.table, 183, 12),
          fontSize: 16,
          fontWeight: 600
        },
        draggable: true,
        name: 'titleBar'
      })

      return group
    },
    getAnchorPoints() {
      return [
        [0, 0],
        [1, 0]
      ]
    }
  })

  registerNode('dice-er-center', {
    draw(cfg, group) {
      let { collapsed = true, mainColor, boxType, icon } = cfg

      let width = 215,
          height = 200
      const boxStyle = {
        stroke: mainColor,
        radius: 2
      }

      if (collapsed === false) {
        height = 200
      } else {
        height = 60
      }

      // 标题蓝色背景
      group.addShape('rect', {
        attrs: {
          fill: boxStyle.stroke,
          height: 30,
          width,
          radius: [boxStyle.radius, boxStyle.radius, 0, 0]
        },
        draggable: true,
        name: 'titleBar'
      })

      return group
    },
    getAnchorPoints() {
      return [
        [0, 0],
        [1, 0]
      ]
    }
  })

  const container = document.getElementById('container')

  const width = container.scrollWidth
  const height = (container.scrollHeight || 700) - 20
  graph = new G6.Graph({
    container: 'container',
    width,
    height,
    defaultNode: {
      size: [270, 50],
      type: 'dice-er-box',
      color: '#5B8FF9',
      style: {
        fill: '#9EC9FF',
        lineWidth: 3
      },
      labelCfg: {
        style: {
          fill: 'black',
          fontSize: 20
        }
      }
    },
    defaultEdge: {
      type: 'dice-er-edge',
      style: {
        stroke: '#e2e2e2',
        lineWidth: 4,
        endArrow: false
      }
    },
    defaultCombo: {
      type: 'circle',
      style: {
        lineWidth: 1,
      },
      labelCfg: {
        refY: 15,
        position: 'bottom',
      },
    },
    comboStateStyles: {
      dragenter: {
        lineWidth: 4,
        stroke: '#FE9797',
      },
    },
    modes: {
      default: ['zoom-canvas', 'drag-node', 'drag-canvas']
    },
    layout: {
      type: 'dagre',
      rankdir: 'LR',
      align: 'UL',
      controlPoints: true,
      nodesepFunc: () => 0.2,
      ranksepFunc: () => 20,
      // nodesepFunc: () => 0,
      // ranksepFunc: (d) => {
      //   if (d.comboId) {
      //     return 1000;
      //   }
      //   return 0.2;
      // },
    },
    animate: true,

    fitCenter: true,
    // 适应视图的大小
    fitView: true,
    // 添加渲染视图 padding
    fitViewPadding: true
  })

  graph.data(data)

  graph.render()
};


nextTick(()=>{
  initGraph(graphData)
})

</script>

<style scoped>

#container {
  position: absolute;
  left: 19px;
  right: 19px;
  bottom: 24px;
  top: 106px;
  margin: auto;
  width: 1342px;
  height: 440px;
  background: linear-gradient(328deg, #4C74FB 0%, #020C20 100%);
  backdrop-filter: blur(2px);
}

</style>
