<template>
  <div id="container"></div>
</template>

<script setup>
import G6 from '@antv/g6/dist/g6.min'

const { getLabelPosition, transform } = G6.Util;

const data = {
    nodes:[
      {
        id:'node1',
        label:'Node 1',
        x:150,
        y:150,
        centerColor:"#bae7ff",
        details:[
          {cat:'pv',values :[20,30,40,30,30] , color:'#5B8FF9'},
          {cat:'dal',values :[40,30,20,30,50] , color:'#5AD8A6'},
          {cat:'uv',values :[40,30,30,40,40] , color:'#5D7092'},
          {cat:'sal',values :[20,30,50,20,20] , color:'#F6BD16'},
          {cat:'cal',values :[10,10,20,20,20] , color:'#E8684A'},
        ]
      },
      {
        id:'node2',
        label:'Node 2',
        x:400,
        y:150,
        centerColor:"#bae7ff",
        details:[
          {cat:'pv',values :[20,30,40,30,30] , color:'#5B8FF9'},
          {cat:'dal',values :[40,30,20,30,50] , color:'#5AD8A6'},
          {cat:'uv',values :[40,30,30,40,40] , color:'#5D7092'},
          {cat:'sal',values :[20,30,50,20,20] , color:'#F6BD16'},
          {cat:'cal',values :[10,10,20,20,20] , color:'#E8684A'},
        ]
      }
    ],
}

const registerNode = ()=>{
  G6.registerNode('lineNode',{
    drawText(cfg,group){
      group.addShape('text',{
        attrs : {
          // text:cfg.label,
          // fill:'white',
          // ...cfg.labelCfg.style
        }
      })
    },
    draw(cfg,group){
      //1.
      const attrs = cfg;
      group.addShape(
          'circle',//继承内置节点的shape，可选rect,circle,ellipse,path,
          {
            attrs:{
              r:70,
              x:0,
              y:0,
              opacity:0
            }
          })
      const rGap = 10 ;
       let currentR = 30;
      for (let i = 0; i < 5; i++) {
        group.addShape('circle',{
          attrs:{
            r:currentR,
            x:0,
            y:0,
            lineWidth:1,
            stroke:"#bae7ff",
            lineDash:[5,5]
          }
        })
        currentR += rGap;
      }
      group.addShape('circle',{
        attrs:{
          fill:cfg.centerColor,
          x:0,
          y:0,
          r:20
        }
      })
      group.addShape('text',{
        attrs:{
          text:cfg.label,
          fill:'#fff',
          x:0,
          y:0,
          textAlign:'center',
          textBaseline:'middle',
          fontWeight:500,
        },
        name:'line-chart-label'
      })
      const alphaGap  = Math.PI*2/25;
      let pointCount = 0;
      cfg.details.forEach(detail=>{
        const {color,values} = detail;
        const positions = [];
        values.forEach(value=>{
            const r = value + 20;
            const alpha = alphaGap* pointCount;
            pointCount++;
            const x = r*Math.cos(alpha);
            const y = r*Math.sin(alpha);
          const point =  group.addShape('circle',{
              attrs:{
                x,
                y,
                r:2,
                fill:color,
              },
            name:"line-chart-point"
            });
          positions.push({x,y})
        })
        group.addShape('path',{
          attrs:{
            path:positions.map((pos,i)=>[i===0 ? 'M':'L',pos.x , pos.y]),
            stroke:color,
            lineWidth:1
          },
          name:'keke-path'
        })
      })
      const shape = group
      this.drawText(cfg,group)
      return shape
    },
    afterDraw(cfg, group){
      const textShape = group.find(ele=> ele.get('name') === 'line-chart-label');
      const  rate = 1.01
      textShape.animate(ratio=>{
        const currentFontSize = textShape.attr('fontSize')||15
        const scale = ratio<0.5 ? rate : (1/rate)
        const targetFontSize = currentFontSize*scale;
        return {fontSize: targetFontSize}
      },{
        duration:1000,
        repeat:true,
      })

      const shape = group.find(ele=> ele.get('name') === 'keke-path');



      const arrow = group.addShape("marker", {
        attrs: {
          x: 16,
          y: 0,
          r: 8,
          lineWidth: 2,
          stroke: "#3370ff",
          fill: "#fff",
          symbol: (x, y, r) => {
            return [
              ["M", x - 6, y - 4],
              ["L", x - 2, y],
              ["L", x - 6, y + 4]
            ];
          }
        }
      });

      // animation for the red circle
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
            // repeat: true, // Whether executes the animation repeatly
            duration: 3000 // the duration for executing once
          }
      );

    },
    update(){

    },
    afterUpdate(){

    },
    setState(){

    },
    getAnchorPoints(){

    },
  },'extendNodeName')
}

const initTree = (data)=> {
  registerNode();
  const container = document.getElementById('container');
  const width = container.scrollWidth || 800;
  const height = container.scrollHeight || 800;
  const graph = new G6.Graph({
    // 控件
    container :container,
    // 宽度
    width:width,
    // 高度
    height:height,
    // 布局
    layout:{
      type:'degree',
    },
    // 节点
    defaultNode: {
      type:  'lineNode',
      style: {
        width:  180,
        height: 60,
        radius: 10,
      },
      labelCfg: {
        style: {
          fontSize: 20,
        },
      },
      anchorPoints: [
        [0, 0],
        [1, 0],
        [0, 1],
        [1, 1],
      ],
    },
    fitCenter: true,
    // 边
    defaultEdge:{
      type:'polyline'
    },
    // 适应视图的大小
    fitView:true,
    // 添加渲染视图 padding
    fitViewPadding:true,
    modes:{
      default:['zoom-canvas','drag-node','drag-canvas','brush-node'],
    },
    nodeStateStyles:{
      hover:{
        fill:'lightsteelblue'
      },
      click:{
        stroke:'steelblue',
        lineWidth:10
      }
    }
  });

  graph.read(data);

  graph.on('canvas:click',event=>{
    console.log(event)
  })

  graph.on('node:click',event=>{
    console.log(event)
    graph.setItemState(event.item, 'click' , true);
  })

  graph.on('node:mouseenter',event=>{
    console.log(event)
  })

  graph.on('line-chart-point:mouseenter',e=>{
    const {target} = e;
    target.animate({r:5},{
      duration:200,
      repeat:false
    })
  })

  graph.on('line-chart-point:mouseleave',e=>{
    const {target} = e;
    target.animate({r:2},{
      duration:200,
      repeat:false
    })
  })


}

nextTick(()=>{
  initTree(data)
})

</script>

<style scoped>
#container{
  width: 100%;
  height: 800px;
}
</style>
