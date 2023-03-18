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
  "nodes": [{
    "id": "node1",
    "label": "Node 1",
    "x": 549,
    "y": 320,
    "total": 238,
    "centerImg": "data:image/png;base64,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",
    "resourceType": 1,
    "centerColor": "#bae7ff",
    "details": [{
      "cat": "pv",
      "values": [{
        "text": "学工系统",
        "mainColor":'#F0AC26',
        "image":'/src/assets/images/antv/g6/yellow-r.png'
      }, {
        "text": "学工系统",
        "mainColor":'#F0AC26',
        "image":'/src/assets/images/antv/g6/yellow-l.png'
      }, {
        "text": "教务系统",
        "mainColor":'#33A0FF',
        "image":'/src/assets/images/antv/g6/blue-r.png'
      },{
        "text": "教务系统",
        "mainColor":'#33A0FF',
        "image":'/src/assets/images/antv/g6/blue-l.png'
      },  {
        "text": "人事系统",
        "mainColor":'#2ECDE6',
        "image":'/src/assets/images/antv/g6/green-r.png'
      },{
        "text": "人事系统",
        "mainColor":'#2ECDE6',
        "image":'/src/assets/images/antv/g6/green-l.png'
      },{
        "text": "迎新管理系统",
        "mainColor":'#F0AC26',
        "image":'/src/assets/images/antv/g6/yellow-r.png'
      }, {
        "text": "迎新管理系统",
        "mainColor":'#F0AC26',
        "image":'/src/assets/images/antv/g6/yellow-l.png'
      }, {
        "text": "科研管理平台",
        "mainColor":'#33A0FF',
        "image":'/src/assets/images/antv/g6/blue-r.png'
      },{
        "text": "就业管理系统",
        "mainColor":'#F0AC26',
        "image":'/src/assets/images/antv/g6/yellow-l.png'
      },  {
        "text": "智慧办公系统",
        "mainColor":'#2ECDE6',
        "image":'/src/assets/images/antv/g6/green-r.png'
      },{
        "text": "智慧办公系统",
        "mainColor":'#2ECDE6',
        "image":'/src/assets/images/antv/g6/green-l.png'
      },  {
        "text": "学生公寓管理系统",
        "mainColor":'#2ECDE6',
        "image":'/src/assets/images/antv/g6/green-r.png'
      },{
        "text": "学生公寓管理系统",
        "mainColor":'#2ECDE6',
        "image":'/src/assets/images/antv/g6/green-l.png'
      }, {
        "text": "科研管理平台",
        "mainColor":'#33A0FF',
        "image":'/src/assets/images/antv/g6/blue-l.png'
      } ],
      "color": "#5B8FF9"
    }],
    "type": "lineNode",
    "style": {
      "hover": {
        "fill": "lightsteelblue"
      },
      "click": {
        "stroke": "steelblue",
        "lineWidth": 10
      },
      "width": 180,
      "height": 60,
      "radius": 10
    },
    "labelCfg": {
      "style": {
        "fontSize": 20
      }
    },
    "anchorPoints": [
      [0, 0],
      [1, 0],
      [0, 1],
      [1, 1]
    ]
  }],
  "edges": [{
    "source": "node1",
    "sourceKey": "source-10086",
    "target": "node2",
    "targetKey": "target-10086",
    "type": "dice-er-edge",
    "style": {
      "stroke": "#e11d1d",
      "lineWidth": 4,
      "endArrow": false
    }
  }]
}
const  initGraph = (data)=> {
      const container = document.getElementById('container')
      const width = container.scrollWidth || 800
      const height = container.scrollHeight || 800

      const registerNode = () => {
        G6.registerEdge('dice-er-edge', {
          afterDraw(cfg, group) {
            {
              const shape = group.get('children')[0]
              let index = 0
              // Define the animation
              const lineDash = [4, 2, 1, 2]
              shape.animate(
                  () => {
                    index++
                    if (index > 9) {
                      index = 0
                    }
                    const res = {
                      lineDash,
                      lineDashOffset: -index
                    }
                    // returns the modified configurations here, lineDash and lineDashOffset here
                    return res
                  },
                  {
                    repeat: true, // whether executes the animation repeatly
                    duration: 3000 // the duration for executing once
                  }
              )
            }

            // get the first shape in the group, it is the edge's path here=
            // const shape = group.get('children')[0];
            // let index = 0;
            // // Define the animation
            // const lineDash = [4, 2, 1, 2];
            // shape.animate(
            //     () => {
            //       index++;
            //       if (index > 9) {
            //         index = 0;
            //       }
            //       const res = {
            //         lineDash,
            //         lineDashOffset: -index,
            //       };
            //       // returns the modified configurations here, lineDash and lineDashOffset here
            //       return res;
            //     },
            //     {
            //       repeat: true, // whether executes the animation repeatly
            //       duration: 3000, // the duration for executing once
            //     },
            // );
          }
        })
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
                })
              },
              draw(cfg, group) {
                //1.
                const attrs = cfg

                const contentWidth = 428
                const contentHeight = 356
                group.addShape('image', {
                  attrs: {
                    x: -contentWidth / 2,
                    y: -contentHeight / 2 + 18 ,
                    height: contentHeight,
                    width: contentWidth,
                    img: '/src/assets/images/antv/g6/center-bg.png',
                    cursor: 'pointer'
                  },
                  name: 'center-img',
                  draggable: true
                })
                const image = group.addShape('image', {
                  attrs: {
                    x: -248 / 2  ,
                    y: -248 / 2  ,
                    height: 248,
                    width: 248,
                    img: '/src/assets/images/antv/g6/circle-bg.png',
                    cursor: 'pointer'
                  },
                  name: 'center-img',
                  draggable: true
                })
                image.animate(
                    (ratio) => {
                      const toMatrix = G6.Util.transform(
                          [0, 1, 1, 1, 0, 1, 1, 1, 0],
                          [['r', ratio * Math.PI * 2]],
                      );
                      return {
                        matrix: toMatrix,
                      };
                    },
                    {
                      repeat: true,
                      duration: 3000,
                      easing: 'easeCubic',
                    },
                );


                let pointCount = 0
                cfg.details.forEach(detail => {
                  const { color, values } = detail
                  values.forEach((value, index) => {
                    const positions = []
                    pointCount++


                    {
                      const itemHeight = 26
                      const yArr = [
                          0,
                        itemHeight * -4,
                        itemHeight * -4,
                        0,
                        0,
                        itemHeight * 4,
                        itemHeight * 4,


                        itemHeight * -6,
                        itemHeight * -6,
                        itemHeight * -2,
                        itemHeight * -2,

                        itemHeight * 2,
                        itemHeight * 2,
                        itemHeight * 6,
                        itemHeight * 6,

                        itemHeight * 1.8,


                        // itemHeight*7,itemHeight*6,itemHeight*5,itemHeight*4,itemHeight*3,itemHeight*2,36,0,-36 ,itemHeight*(-2) ,itemHeight*(-3) ,itemHeight*(-4) ,itemHeight*(-5) ,itemHeight*(-6) ,itemHeight*(-7) ,
                      ]
                      const rMin = 400,
                          rMax = 450
                      const itemX = 50 ;
                      const xArr = [
                        itemX*-8,
                        itemX*8,
                        itemX*-8,
                        itemX*8,
                        itemX*-8,
                        itemX*8,
                        itemX*-8,
                        itemX*8.5,
                        itemX*-7,
                        itemX*10,
                        itemX*-10,
                        itemX*8.5,
                        itemX*-10,
                        itemX*8.5,
                        itemX*-7,
                        itemX*-7,
                      ]
                      const boxWidth = 155
                      const boxHeight = 28
                      const y = yArr[pointCount % yArr.length]
                      const x = xArr[pointCount % xArr.length]
                      let r =  Math.sqrt(x * x + y * y)
                      // x = x > 0 ? x + 48 : x - 48

                      group.addShape('image', {
                        attrs: {
                          x: x > 0 ? x - boxWidth/2 : x - boxWidth/2,
                          y: y - boxHeight / 2,
                          height: 40,
                          width: boxWidth,
                          img: value.image
                        },
                        draggable: true
                      })



                      group.addShape('text', {
                        attrs: {
                          text: fittingString(`${value.text}`, 120, 12),
                          // text:`${x}-${pointCount}`,
                          fill: `${value.mainColor}`,
                          width: boxWidth,
                          height: boxHeight,
                          x: x > 0 ? x - boxWidth/2 +20 : x + boxWidth/2 - 20,
                          y: y + 22 - boxHeight / 2,
                          textAlign: x > 0 ? 'start' : 'end',
                          textBaseline: 'middle',
                          fontSize: 12,
                          fontWeight: 600
                        },
                        name: `source-${value}`
                      })


                      positions.push({ x: x > 0 ? x - 80   : x + 80  , y: y + 23 - boxHeight / 2 })


                      {
                        const y = yArr[pointCount % yArr.length] * 0.35
                        const r = 230
                        let x = pointCount % 2 ? Math.sqrt(r * r - y * y) : -1 * Math.sqrt(r * r - y * y)
                        x = x > 0 ? x + 150 : x - 150

                        positions.push({
                          x: x > 0 ? x - 180: x + 180,
                          y: y + 23 - boxHeight / 2
                        })
                      }

                      {
                        const startPoint = positions[0],
                            endPoint = positions[1]

                        const startPoint2 = {
                              x: 0,
                              y: startPoint.y
                            },
                            endPoint2 = {
                              x: 0,
                              y: endPoint.y
                            }

                        {
                          const _r = 345
                          const xDiff = Math.sqrt(_r * _r - startPoint2.y * startPoint2.y)
                          startPoint2.x = startPoint.x > 0 ? xDiff : -xDiff
                        }

                        {
                          const _r = 250
                          const xDiff = Math.sqrt(_r * _r - endPoint2.y * endPoint2.y)
                          endPoint2.x = endPoint.x > 0 ? xDiff : -xDiff
                        }

                        console.log(startPoint.x)
                        console.log(endPoint.x)

                        if(index<6){

                          group.addShape('path', {
                            attrs: {
                              stroke:
                                  startPoint.x > 0 ? `l(0) 0:${value.mainColor}05  1:${value.mainColor}60 ` : `l(0) 0:${value.mainColor}60  1:${value.mainColor}05 `,
                              lineWidth: 18,
                              path: [
                                ['M', startPoint.x, startPoint.y],
                                [
                                  "C",
                                  startPoint.x*0.6,
                                  startPoint.y,
                                  (startPoint.x*0.6+endPoint.x*1.3)/2,
                                  endPoint.y,
                                  endPoint.x*0.8,
                                  endPoint.y,
                                ],
                                ['L', endPoint.x*0.8, endPoint.y],
                              ],
                              cursor: 'pointer'
                            },
                            name: `path-shape${index}`
                          })


                          group.addShape('path', {
                            attrs: {
                              stroke:
                                  startPoint.x > 0 ? `l(0) 0:${value.mainColor}  1:${value.mainColor} ` : `l(0) 0:${value.mainColor}  1:${value.mainColor} `,
                              lineWidth: 18,
                              path: [
                                ['M', startPoint.x, startPoint.y],
                                ['L', startPoint.x > 0 ? startPoint.x + 4 : startPoint.x-4, startPoint.y],
                              ],
                              cursor: 'pointer'
                            },
                          })
                        }

                      }

                    }
                  })
                })

                this.drawText(cfg, group)
                return group
              },
              afterDraw(cfg, group) {
                console.log(group)
                for (let index = 0; index < 6; index++) {

                  const item = cfg.details[0].values[index]

                  for (let arrowIndex = 0; arrowIndex < 3; arrowIndex++) {
                    setTimeout(()=>{
                      const shape = group.find(ele => {
                        return ele.get('name') === `path-shape${index}`
                      })

                      const arrow = group.addShape('marker', {
                        attrs: {
                          x: 16,
                          y: 0,
                          r: 8,
                          lineWidth: 4,
                          stroke: `${item.mainColor}${30*(3-arrowIndex)}`,
                          fill: '#ffffff00',
                          symbol: (x, y, r) => {
                            return [
                              ['M', x - 6, y - 8],
                              ['L', x +2, y],
                              ['L', x - 6, y + 8]
                            ]
                          }
                        }
                      })
                      // animation for the red circle
                      arrow.animate(
                          ratio => {
                            // the operations in each frame. Ratio ranges from 0 to 1 indicating the prograss of the animation. Returns the modified configurations
                            // get the position on the edge according to the ratio
                            const tmpPoint = shape.getPoint(ratio)
                            const pos = getLabelPosition(shape, ratio)
                            let matrix = [1, 0, 0, 0, 1, 0, 0, 0, 1]
                            matrix = transform(matrix, [
                              ['t', -tmpPoint.x, -tmpPoint.y],
                              ['r', pos.angle],
                              ['t', tmpPoint.x, tmpPoint.y]
                            ])

                            // returns the modified configurations here, x and y here
                            return {
                              x: tmpPoint.x,
                              y: tmpPoint.y,
                              matrix
                            }
                          },
                          {
                            repeat: true, // Whether executes the animation repeatly
                            duration: 3000 // the duration for executing once
                          }
                      )
                    },500*arrowIndex)
                  }

                }
              },
              update() {},
              afterUpdate() {},
              setState() {},
              getAnchorPoints() {}
            },
            'extendNodeName'
        )
      }

      registerNode()
       graph = new G6.Graph({
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
          default: ['zoom-canvas', 'drag-node', 'drag-canvas', 'brush-node']
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
      })

      graph.read(data)

      graph.on('canvas:click', event => {
        console.log(event)
      })

      graph.on('node:click', event => {
        console.log(event)
        graph.setItemState(event.item, 'click', true)
      })

      graph.on('node:mouseenter', event => {
        console.log(event)
      })

      graph.on('item-content:mouseenter', e => {
        const { target } = e
        target.animate(
            {
              shadowColor: 'rgba(44,104,255,0.3500)',
              shadowBlur: 10
            },
            {
              duration: 0,
              repeat: false
            }
        )
      })

      graph.on('item-content:mouseleave', e => {
        const { target } = e
        target.animate(
            {
              shadowColor: 'rgba(44,104,255,0.0500)',
              shadowBlur: 10
            },
            {
              duration: 0,
              repeat: false
            }
        )
      })

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
