<template>
  <div id="container"></div>
</template>

<script setup>
import G6 from '@antv/g6/dist/g6.min'


const initTree = ()=> {


  const {
    Util,
    registerBehavior,
    registerEdge,
    registerNode
  } = G6;

  const rawData = [
      {
    "id": "info",
    "label": "Employee",
    "icon":{
      show:true,
      img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
    },
     "mainColor":'#2C68FF',
     "titleBox":'主',
    "attrs": [
        {
      "key": "id",
      "type": "number(6)"
    },
      {
        "key": "key",
        "type": "varchar(255)"
      },
      {
        "key": "gender",
        "type": "enum(M, F)"
      },
      {
        "key": "birthday",
        "type": "date"
      },
      {
        "key": "hometown",
        "type": "varchar(255)"
      },
      {
        "key": "country",
        "type": "varchar(255)"
      },
      {
        "key": "nation",
        "type": "varchar(255)"
      },
      {
        "key": "country2",
        "type": "varchar(255)"
      },
      {
        "key": "nation2",
        "type": "varchar(255)"
      },
      {
        "key": "country3",
        "type": "varchar(255)"
      },
      {
        "key": "nation3",
        "type": "varchar(255)"
      },
      {
        "key": "jobId",
        "type": "number(3)",
        "relation": [{
          "key": "id",
          "nodeId": "job"
        },]
      },
      {
        "key": "phone",
        "type": "varchar(255)"
      },
      {
        "key": "deptId",
        "type": "number(6)",
        "relation": [{
          "key": "id",
          "nodeId": "dept"
        },{
          "key": "id",
          "nodeId": "dept11"
        }]
      },
      {
        "key": "startTime",
        "type": "date"
      },
      {
        "key": "leaveTime",
        "type": "date"
      }
    ],
  },
    {
      "id": "dept",
      "label": "Department",
      "icon":{
        show:true,
        img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      },
      "mainColor":'#FF7D00',
      "titleBox":'标',
      "attrs": [
          {
        "key": "id",
        "type": "number(6)"
      },
        {
          "key": "title",
          "type": "varchar(255)"
        },
        {
          "key": "desc",
          "type": "text"
        },
        {
          "key": "parent",
          "type": "number(6)",
          "relation": [{
            "key": "id",
            "nodeId": "dept"
          },{
            "key": "id",
            "nodeId": "dept1"
          }]
        },
        {
          "key": "manager",
          "type": "number(6)"
        }
      ]
    },
    {
      "id": "dept11",
      "label": "Department11",
      "icon":{
        show:true,
        img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      },
      // "flexible":{
      //   icon:{
      //     img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      //   },
      // },
      "mainColor":'#FF7D00',
      "titleBox":'标',
      "attrs": [
        {
          "key": "id",
          "type": "number(6)"
        },
        {
          "key": "title",
          "type": "varchar(255)"
        },
        {
          "key": "desc",
          "type": "text"
        },
        {
          "key": "parent",
          "type": "number(6)",
          "relation": [{
            "key": "id",
            "nodeId": "dept2"
          }]
        },
        {
          "key": "manager",
          "type": "number(6)"
        }
      ]
    },
    {
      "id": "dept2",
      "label": "Department2",
      "icon":{
        show:true,
        img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      },
      // "flexible":{
      //   icon:{
      //     img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      //   },
      // },
      "mainColor":'#FF7D00',
      "titleBox":'标',
      "attrs": [
        {
          "key": "id",
          "type": "number(6)"
        },
        {
          "key": "title",
          "type": "varchar(255)"
        },
        {
          "key": "desc",
          "type": "text"
        },
        {
          "key": "parent",
          "type": "number(6)",
          "relation": [{
            "key": "id",
            "nodeId": "dept3"
          }]
        },
        {
          "key": "manager",
          "type": "number(6)"
        }
      ]
    },
    {
      "id": "dept3",
      "label": "Department3",
      "icon":{
        show:true,
        img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      },
      // "flexible":{
      //   icon:{
      //     img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      //   },
      // },
      "mainColor":'#FF7D00',
      "titleBox":'标',
      "attrs": [
        {
          "key": "id",
          "type": "number(6)"
        },
        {
          "key": "title",
          "type": "varchar(255)"
        },
        {
          "key": "desc",
          "type": "text"
        },
        {
          "key": "parent",
          "type": "number(6)",
          "relation": [{
            "key": "id",
            "nodeId": "dept4"
          }]
        },
        {
          "key": "manager",
          "type": "number(6)"
        }
      ]
    },
    {
      "id": "dept4",
      "label": "Department4",
      "icon":{
        show:true,
        img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      },
      // "flexible":{
      //   icon:{
      //     img:'https://g.alicdn.com/cm-design/arms-trace/1.0.155/styles/armsTrace/images/TAIR.png'
      //   },
      // },
      "mainColor":'#FF7D00',
      "titleBox":'标',
      "attrs": [
        {
          "key": "id",
          "type": "number(6)"
        },
        {
          "key": "title",
          "type": "varchar(255)"
        },
        {
          "key": "desc",
          "type": "text"
        },
        {
          "key": "parent",
          "type": "number(6)",
        },
        {
          "key": "manager",
          "type": "number(6)"
        }
      ]
    },
  ]

  const isInBBox = (point, bbox) => {
    const {
      x,
      y
    } = point;
    const {
      minX,
      minY,
      maxX,
      maxY
    } = bbox;

    return x < maxX && x > minX && y > minY && y < maxY;
  };

  const itemHeight = 32;
  registerBehavior("dice-er-scroll", {
    getDefaultCfg() {
      return {
        multiple: true,
      };
    },
    getEvents() {
      return {
        itemHeight,
        wheel: "scorll",
        click: "click",
        "node:mousemove": "move",
      };
    },
    scorll(e) {
      e.preventDefault();
      const {
        graph
      } = this;
      const nodes = graph.getNodes().filter((n) => {
        const bbox = n.getBBox();

        return isInBBox(graph.getPointByClient(e.clientX, e.clientY), bbox);
      });

      const x = e.deltaX || e.movementX;
      let y = e.deltaY || e.movementY;
      if (!y && navigator.userAgent.indexOf('Firefox') > -1) y = (-e.wheelDelta * 125) / 3

      if (nodes) {
        nodes.forEach((node) => {
          const model = node.getModel();
          if (model.attrs.length < 2) {
            return;
          }
          const idx = model.startIndex || 0;
          let startX = model.startX || 0.5;
          let startIndex = idx + y * 0.02;
          startX -= x;
          if (startIndex < 0) {
            startIndex = 0;
          }
          if (startX > 0) {
            startX = 0;
          }
          if (startIndex > model.attrs.length - 1) {
            startIndex = model.attrs.length - 1;
          }
          graph.update(node, {
            startIndex,
            startX,
          });
        });
      }


    },
    click(e) {
      const {
        graph
      } = this;
      const {
        y
      } = e;
      const item = e.item;
      const shape = e.shape;


      if (!item) {
        return;
      }
      const model = item.getModel();

      if (shape.get("name") === "collapse") {
        graph.updateItem(item, {
          collapsed: true,
          size: [300, 50],
        });
        // setTimeout(() => graph.layout(), 100);
      } else if (shape.get("name") === "expand") {
        graph.updateItem(item, {
          collapsed: false,
          size: [300, 80],
        });
        // setTimeout(() => graph.layout(), 100);
      }


      // 吴柯

      const name = e.shape.get("name");

      if (name && name.startsWith("item")) {
        this.graph.updateItem(item, {
          selectedIndex: Number(name.split("-")[1]),
          selectedLabel: e.shape.get('label'),
        });
      }

    },
    move(e) {
      console.log('移动', e.shape.get("name"))
      const name = e.shape.get("name");
      const item = e.item;

      if (name && name.startsWith("item")) {
        // this.graph.updateItem(item, {
        //   selectedIndex: Number(name.split("-")[1]),
        // });
      } else {
        // this.graph.updateItem(item, {
        //   selectedIndex: NaN,
        // });
      }
    },
  });

  registerEdge("dice-er-edge", {
    draw(cfg, group) {
      const edge = group.cfg.item;
      const sourceNode = edge.getSource().getModel();
      const targetNode = edge.getTarget().getModel();


      const sourceIndex = sourceNode.attrs.findIndex(
          (e) => e.key === cfg.sourceKey
      );

      const sourceStartIndex = sourceNode.startIndex || 0;

      let sourceY = 15;

      // if (!sourceNode.collapsed && sourceIndex > sourceStartIndex - 1) {
      //   sourceY = 30 + (sourceIndex - sourceStartIndex + 0.5) * 30;
      //   sourceY = Math.min(sourceY, 80);
      // }

      const targetIndex = targetNode.attrs.findIndex(
          (e) => e.key === cfg.targetKey
      );

      const targetStartIndex = targetNode.startIndex || 0;

      let targetY = 15;

      // if (!targetNode.collapsed && targetIndex > targetStartIndex - 1) {
      //   targetY = (targetIndex - targetStartIndex + 0.5) * 30 + 30;
      //   targetY = Math.min(targetY, 80);
      // }

      const startPoint = {
        ...cfg.startPoint
      };
      const endPoint = {
        ...cfg.endPoint
      };

      startPoint.y = startPoint.y + sourceY;
      endPoint.y = endPoint.y + targetY;

      let shape;
      if (sourceNode.id !== targetNode.id) {
        shape = group.addShape("path", {
          attrs: {
            path: [
              ["M", startPoint.x, startPoint.y],
              [
                "C",
                endPoint.x / 3 + (2 / 3) * startPoint.x,
                startPoint.y,
                endPoint.x / 3 + (2 / 3) * startPoint.x,
                endPoint.y,
                endPoint.x,
                endPoint.y,
              ],
            ],
            stroke: `l(0) 0:${sourceNode.mainColor}50  1:${targetNode.mainColor}99 `,
            lineWidth: 2,
            endArrow: false,
          },
          name: "path-shape",
        });
      } else if (!sourceNode.collapsed) {
        let gap = Math.abs((startPoint.y - endPoint.y) / 3);
        if (startPoint["index"] === 1) {
          gap = -gap;
        }
        shape = group.addShape("path", {
          attrs: {
            path: [
              ["M", startPoint.x, startPoint.y],
              [
                "C",
                startPoint.x - gap,
                startPoint.y,
                startPoint.x - gap,
                endPoint.y,
                startPoint.x,
                endPoint.y,
              ],
            ],
            stroke: `l(0) 0:${sourceNode.mainColor}50  1:${targetNode.mainColor}99 `,
            lineWidth: 2,
            endArrow: false,
          },
          name: "path-shape",
        });
      }

      return shape;
    },
    afterDraw(cfg, group) {
      const labelCfg = cfg.labelCfg || {};
      const edge = group.cfg.item;
      const sourceNode = edge.getSource().getModel();
      const targetNode = edge.getTarget().getModel();
      if (sourceNode.collapsed && targetNode.collapsed) {
        return;
      }
      const path = group.find(
          (element) => element.get("name") === "path-shape"
      );

      const labelStyle = Util.getLabelPosition(path, 0.5, 0, 0, true);
      const label = group.addShape("text", {
        attrs: {
          ...labelStyle,
          text: cfg.label || '',
          fill: "#000",
          textAlign: "center",
          stroke: "#fff",
          lineWidth: 1,
        },
      });
      label.rotateAtStart(labelStyle.rotate);
    },
  });

  registerNode("dice-er-box", {
    draw(cfg, group) {

      const {
        attrs = [],
        startIndex = 0,
        selectedIndex,
        selectedLabel,
        collapsed,
        icon,
        mainColor,
        titleBox
      } = cfg;

      const width = 215;
      const height = 200;
      const itemCount = 7;
      const boxStyle = {
        stroke: mainColor,
        radius: 2,
      };

      const list = attrs;
      const afterList = list.slice(
          Math.floor(startIndex),
          Math.floor(startIndex + itemCount - 1)
      );
      const offsetY = (0.5 - (startIndex % 1)) * itemHeight + 30;

      // 标题蓝色背景
      group.addShape("rect", {
        attrs: {
          fill: boxStyle.stroke,
          height: 30,
          width,
          radius: [boxStyle.radius, boxStyle.radius, 0, 0],
        },
        draggable: true,
      });


      let fontLeft = 12;
      // 标题左侧图片
      if (titleBox ) {
        group.addShape("rect", {
          attrs: {
            x: 7,
            y: 3,
            fill: "#fff",
            height: 24,
            width:24,
            radius: [4, 4, 4, 4],
          },
          draggable: true,
        });
        group.addShape("text", {
          attrs: {
            x: 13,
            y: 22,
            height: 16,
            width: 16,
            fill: mainColor,
            text: titleBox,
            fontSize: 12,
            fontWeight: 500,
          },
          draggable: true,
        });

        // group.addShape("image", {
        //   attrs: {
        //     x: 8,
        //     y: 8,
        //     height: 16,
        //     width: 16,
        //     ...icon,
        //   },
        // });
        fontLeft += 25;
      }


      // 标题文字
      group.addShape("text", {
        attrs: {
          y: 22,
          x: fontLeft,
          fill: "#fff",
          text: cfg.label,
          fontSize: 12,
          fontWeight: 500,
        },
        draggable: true,
      });



      // 标题右侧图片
      group.addShape("image", {
        attrs: {
          x: 190,
          y: 8,
          height: 16,
          width: 16,
          img:collapsed ? "data:image/png;base64,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9csA+G3gmuZrXR0gOWrQjesjQcbxahpN0t8f9/fIx7TdAI4xs2vHdFJsHQEJUsetWa+y5cKbAPh77svaVwCcaGa+PpfaighIkBWBHjoMyfsC2AHAN/Lxf/ve7FcDuArAJWb266Ex9Hl7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0JSJD2TDViIgISJFExlUp7AhKkPVONmIiABElUTKXSnoAEac9UIyYiIEESFVOptCcgQdoz1YiJCEiQRMVUKu0J/B/3vPznmxFXMgAAAABJRU5ErkJggg=="
              : "data:image/png;base64,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",
          cursor: "pointer",
        },
        name: collapsed ? "expand" : "collapse",
        draggable: true,
      });


      group.addShape("circle", {
        attrs: {
          x: 0,
          y: 16,
          r: 3,
          stroke: 'white',
          fill: mainColor,
          radius: 2,
          lineWidth: 1,
          cursor: "pointer",
        },
      });

      // // 底部伸缩背景
      // group.addShape("rect", {
      //   attrs: {
      //     x: 0,
      //     y: collapsed ? 30 : 80,
      //     height: 15,
      //     width,
      //     fill: "#f5eeee",
      //     radius: [0, 0, boxStyle.radius, boxStyle.radius],
      //     cursor: "pointer",
      //   },
      //   name: collapsed ? "expand" : "collapse",
      // });
      //
      // group.addShape("text", {
      //   attrs: {
      //     x: width / 2 - 6,
      //     y: (collapsed ? 30 : 80) + 12,
      //     text: collapsed ? "+" : "-",
      //     width,
      //     fill: "#000",
      //     radius: [0, 0, boxStyle.radius, boxStyle.radius],
      //     cursor: "pointer",
      //   },
      //   name: collapsed ? "expand" : "collapse",
      // });

      //最外边框
     group.addShape("rect", {
        attrs: {
          x: 0,
          y: 0,
          width,
          height: collapsed ? 30 : height,
          ...boxStyle,
          shadowColor:mainColor,
          shadowBlur:10,
        },
        draggable: true,
      });

      const keyshape = group;

      if (collapsed) {
        return keyshape;
      }

      const listContainer = group.addGroup({});
      listContainer.setClip({
        type: "rect",
        attrs: {
          x: -8,
          y: 30,
          width: width + 16,
          height: height-32,
        },
      });
      listContainer.addShape({
        type: "rect",
        attrs: {
          x: 1,
          y: 30,
          width: width - 2,
          height: height-32,
          fill: "#fff",
        },
        draggable: true,
      });




      if (list.length > itemCount) {
        const barStyle = {
          width: 5,
          padding: 0,
          boxStyle: {
            stroke: "#00000088",
          },
          innerStyle: {
            fill: "#00000088",
          },
        };

        // 滚轮竖线
        // listContainer.addShape("rect", {
        //   attrs: {
        //     y: 30,
        //     x: width - barStyle.padding - barStyle.width,
        //     width: barStyle.width,
        //     height: height - 30,
        //     ...barStyle.boxStyle,
        //   },
        // });

        const indexHeight =
            afterList.length > itemCount ?
                (afterList.length / list.length) * height :
                10;

        // 滚轮柱
        listContainer.addShape("rect", {
          attrs: {
            y: 30 +
                barStyle.padding +
                (startIndex / list.length) * (height - 30),
            x: width - barStyle.padding - barStyle.width,
            width: barStyle.width,
            height: Math.min(height, indexHeight),
            ...barStyle.innerStyle,

          },
        });
      }
      if (afterList) {
        afterList.forEach((e, i) => {
          let {
            key = "", type
          } = e;
          // if (type) {
          //   key += " - " + type;
          // }
          const label = key.length > 26 ? key.slice(0, 24) + "..." : key;


          const isSelected =
              (Math.floor(startIndex) + i === Number(selectedIndex))
              || selectedLabel === label;

          listContainer.addShape("rect", {
            attrs: {
              x: 7,
              y: i * itemHeight - itemHeight / 2 + offsetY,
              width: width - 14,
              height: itemHeight,
              radius: 2,
              lineWidth: 1,
              cursor: "pointer",
              fill: isSelected? mainColor:'#fff',
              opacity:0.1,
            },
            name: `item-${Math.floor(startIndex) + i}-content`,
            label:label,
            draggable: true,
          });

          if (!cfg.hideDot) {
            // listContainer.addShape("circle", {
            //   attrs: {
            //     x: 0,
            //     y: i * itemHeight + offsetY,
            //     // r: 3,
            //     r: 3,
            //     stroke: boxStyle.stroke,
            //     fill: "white",
            //     radius: 2,
            //     lineWidth: 1,
            //     cursor: "pointer",
            //   },
            // });
            // listContainer.addShape("circle", {
            //   attrs: {
            //     x: width,
            //     y: i * itemHeight + offsetY,
            //     r: 3,
            //     // r: 0,
            //     stroke: boxStyle.stroke,
            //     fill: "white",
            //     radius: 2,
            //     lineWidth: 1,
            //     cursor: "pointer",
            //   },
            // });
          }

          listContainer.addShape("text", {
            attrs: {
              x: 12,
              y: i * itemHeight + offsetY + 6,
              text: label,
              fontSize: 12,
              fill: mainColor,
              fontFamily: "Avenir,-apple-system,BlinkMacSystemFont,Segoe UI,PingFang SC,Hiragino Sans GB,Microsoft YaHei,Helvetica Neue,Helvetica,Arial,sans-serif,Apple Color Emoji,Segoe UI Emoji,Segoe UI Symbol",
              full: e,
              fontWeight: isSelected ? 600 : 500,
              cursor: "pointer",
            },
            name: `item-${Math.floor(startIndex) + i}`,
            label:label,
          });
        });
      }



      return keyshape;
    },
    getAnchorPoints() {
      return [
        [0, 0],
        [1, 0],
      ];
    },
  });

  const dataTransform = (data) => {
    const nodes = [];
    const edges = [];

    data.map((node) => {
      nodes.push({
        ...node
      });
      if (node.attrs) {
        node.attrs.forEach((attr) => {
          if (attr.relation) {
            attr.relation.forEach((relation) => {
              edges.push({
                source: node.id,
                target: relation.nodeId,
                sourceKey: attr.key,
                targetKey: relation.key,
                label: relation.label,
              });
            });
          }

        });
      }
    });

    return {
      nodes,
      edges,
    };
  }

  const container = document.getElementById('container');

  const width = container.scrollWidth;
  const height = (container.scrollHeight || 500) - 20;
  const graph = new G6.Graph({
    container: 'container',
    width,
    height,
    defaultNode: {
      size: [300, 200],
      type: 'dice-er-box',
      color: '#5B8FF9',
      style: {
        fill: '#9EC9FF',
        lineWidth: 3,
      },
      labelCfg: {
        style: {
          fill: 'black',
          fontSize: 20,
        },
      },
    },
    defaultEdge: {
      type: 'dice-er-edge',
      style: {
        stroke: '#e2e2e2',
        lineWidth: 4,
        endArrow: false,
      },
    },
    modes: {
      default: ['dice-er-scroll', 'drag-node', 'drag-canvas'],
    },
    layout: {
      type: 'dagre',
      rankdir: 'LR',
      align: 'UL',
      controlPoints: true,
      nodesepFunc: () => 0.2,
      ranksepFunc: () => 0.5,
    },
    animate: true,
    fitView: true
  })

  graph.data(dataTransform(rawData));

  graph.render();




}
nextTick(()=>{
  initTree()
})

</script>

<style scoped>
#container{
  width: 100%;
  height: 800px;
}
</style>