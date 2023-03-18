<template>
  <div id="container"></div>
</template>

<script setup>
import G6 from '@antv/g6/dist/g6.min'



const initTree = ()=> {

  const container = document.getElementById('container');
  const width = container.scrollWidth || 800;
  const height = container.scrollHeight || 800;

  const registerNode = ()=>{



    G6.registerEdge("dice-er-edge", {
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
                  lineDashOffset: -index,
                };
                // returns the modified configurations here, lineDash and lineDashOffset here
                return res;
              },
              {
                repeat: true, // whether executes the animation repeatly
                duration: 3000, // the duration for executing once
              },
          );
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

      },
    });
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
        // group.addShape(
        //     'circle',//继承内置节点的shape，可选rect,circle,ellipse,path,
        //     {
        //       attrs:{
        //         r:600,
        //         x:0,
        //         y:0,
        //         opacity:1,
        //         stroke:"#c40d0d"
        //       }
        //     })
        const contentWidth = 484;
        const contentHeight = 534;
        group.addShape('image',{
          attrs:{
            x:-contentWidth/2,
            y:-contentHeight/2 +26,
            height: contentHeight,
            width: contentWidth,
            // img: 'https://img1.baidu.com/it/u=223288250,1771099662&fm=253&fmt=auto&app=138&f=JPEG?w=800&h=500',
            img: 'http://localhost:5173/src/assets/编组 7 (1).png',
            cursor: "pointer",
          },
          name:'center-img',
          draggable: true,
        })
        const alphaGap  = Math.PI*2/25;
        let pointCount = 0;
        cfg.details.forEach(detail=>{
          console.log({pointCount})
          const {color,values} = detail;
          values.forEach((value,index)=>{
            const positions = [];
            const r = 200;
            const yArr = [
              10*(-7) ,10*(-6) , 10*(-5) , 10*(-4) , 10*(-3) , 10*(-2) , -10 ,0, 10,10*2,10*3,10*4,10*5,10*6,10*7,
              10*(-11),10*(-12),10*(-13),10*(-14),10*(-15),10*(-16),10*(-17),10*(-18),10*(-19),10*(-20),10*(-21),10*(-22),10*(-23),10*(-24),10*(-25),10*(-26),10*(-27)
            ];
            const alpha = yArr[pointCount%yArr.length] ;
            pointCount++;
            const x = r*Math.cos(alpha);
            const y = r*Math.sin(alpha);


            // group.addShape("rect", {
            //   attrs: {
            //     x:x-11,
            //     y,
            //     width:22,
            //     height: 14,
            //     stroke:"#FF9595",
            //     fill:'#FF9595',
            //     radius: 9,
            //   },
            //   draggable: true,
            // });
            // group.addShape('text',{
            //   attrs:{
            //     text:'2',
            //     fill:'white',
            //     width:22,
            //     height: 14,
            //     x:x,
            //     y:y+7,
            //     textAlign:"center",
            //     textBaseline:'middle',
            //     fontSize: 10,
            //     fontWeight:500,
            //   },
            //   name:''
            // })


            {
              const itemHeight = 36;
              const yArr = [
                itemHeight*(-7) ,itemHeight*(-6) , itemHeight*(-5) , itemHeight*(-4) , itemHeight*(-3) , itemHeight*(-2) , -1*itemHeight ,0, itemHeight,itemHeight*2,itemHeight*3,itemHeight*4,itemHeight*5,itemHeight*6,itemHeight*7,
                itemHeight*(-7) ,itemHeight*(-6) , itemHeight*(-5) , itemHeight*(-4) , itemHeight*(-3) , itemHeight*(-2) , -1*itemHeight ,0, itemHeight,itemHeight*2,itemHeight*3,itemHeight*4,itemHeight*5,itemHeight*6,itemHeight*7,
                // itemHeight*7,itemHeight*6,itemHeight*5,itemHeight*4,itemHeight*3,itemHeight*2,36,0,-36 ,itemHeight*(-2) ,itemHeight*(-3) ,itemHeight*(-4) ,itemHeight*(-5) ,itemHeight*(-6) ,itemHeight*(-7) ,
              ];
              const rMin = 400 , rMax = 450
              const rArr = [
                rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,
                rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,rMax,rMin,
              ];
              const boxWidth = 192;
              const boxHeight = 28;
              const y = yArr[pointCount % yArr.length] ;
              const r = rArr[pointCount % rArr.length];
              let x = pointCount%2 ?Math.sqrt(r*r - y*y) : -1*Math.sqrt(r*r - y*y)
              x = x> 0 ? x+48 : x-48

              group.addShape("rect", {
                attrs: {
                  x: x > 0 ? x - 100 : x-94,
                  y: y - boxHeight/2,
                  width:192,
                  height: 28,
                  stroke:"white",
                  fill:'#ffffff',
                  shadowColor:'rgba(44,104,255,0.0500)',
                  shadowBlur:10,
                  radius: 2,
                  cursor:'pointer'
                },
                draggable: true,
                name:'item-content'
              });
              group.addShape("rect", {
                attrs: {
                  x: x > 0 ? x - 92 : x+60,
                  y: y+8 - boxHeight/2,
                  width:32,
                  height: 14,
                  stroke:"#5BCFFF",
                  fill:'#5BCFFF',
                  radius: 9,
                },
                draggable: true,
              });
              group.addShape('text',{
                attrs:{
                  text:'9999',
                  fill:'white',
                  height: 28,
                  x: x > 0 ? x - 88 : x+64,
                  y:y+16 - boxHeight/2,
                  textBaseline:'middle',
                  fontSize: 10,
                  fontWeight:500,
                },
                name:''
              })
              group.addShape('text',{
                attrs:{
                  text:`source-${value}`,
                  fill:'#000',
                  height: 28,
                  x: x > 0 ? x -50 : x-30,
                  y:y+16 - boxHeight/2,
                  textBaseline:'middle',
                  fontSize: 12,
                  fontWeight:400,
                },
                name:`source-${value}`,
              })
              group.addShape("rect", {
                attrs: {
                  x: x > 0 ? x +24 : x-80,
                  y: y+8 - boxHeight/2,
                  width:32,
                  height: 14,
                  stroke:"#3FECC1",
                  fill:'#3FECC1',
                  radius: 9,
                },
                draggable: true,
              });
              group.addShape('text',{
                attrs:{
                  text:'9999',
                  fill:'white',
                  height: 28,
                  x: x > 0 ? x +30 : x-75,
                  y:y+16 - boxHeight/2,
                  textBaseline:'middle',
                  fontSize: 10,
                  fontWeight:500,
                },
                name:''
              })
              group.addShape("rect", {
                attrs: {
                  x: x > 0 ? x - 100 : x-94,
                  y: y - boxHeight/2,
                  width:192,
                  height: 28,
                  stroke:"white",
                  fill:'#ffffff00',
                  shadowColor:'rgba(44,104,255,0.0500)',
                  shadowBlur:10,
                  radius: 2,
                  cursor:'pointer'
                },
                draggable: true,
                name:'item-content'
              });

              positions.push({x:x > 0 ? x - 100 : x+100,y:y+16 - boxHeight/2})

              group.addShape("rect", {
                attrs: {
                  x: x > 0 ? x - 128 : x+106,
                  y: y+8 - boxHeight/2,
                  width:22,
                  height: 14,
                  stroke:"#A955EA",
                  fill:'#A955EA',
                  radius: 9,
                },
                draggable: true,
              });
              group.addShape('text',{
                attrs:{
                  text:'2',
                  fill:'white',
                  width:22,
                  height: 14,
                  x: x > 0 ? x - 128 + 12 : x+106 +12,
                  y:y+16 - boxHeight/2,
                  textAlign:"center",
                  textBaseline:'middle',
                  fontSize: 10,
                  fontWeight:500,
                },
                name:''
              })
              group.addShape("rect", {
                attrs: {
                  x: x > 0 ? x - 128 -26 : x+106+26,
                  y: y+8 - boxHeight/2,
                  width:22,
                  height: 14,
                  stroke:"#FFC266",
                  fill:'#FFC266',
                  radius: 9,
                },
                draggable: true,
              });
              group.addShape('text',{
                attrs:{
                  text:'1',
                  fill:'white',
                  width:22,
                  height: 14,
                  x: x > 0 ? x - 128 -26 + 12  : x+106+26 +12,
                  y:y+16 - boxHeight/2,
                  textAlign:"center",
                  textBaseline:'middle',
                  fontSize: 10,
                  fontWeight:500,
                },
                name:''
              })

              {

                const y = yArr[pointCount % yArr.length]*0.69 ;
                const r = 230;
                let x = pointCount%2 ?Math.sqrt(r*r - y*y) : -1*Math.sqrt(r*r - y*y)
                x = x> 0 ? x+ 150: x-150


                group.addShape("image", {
                  attrs: {
                    x: x > 0 ? x - 128 -26 : x+106+26,
                    y: y+8 - boxHeight/2,
                    height: 16,
                    width: 16,
                    img:"data:image/png;base64,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",
                    cursor: "pointer",
                  },
                  draggable: true,
                });
                group.addShape("rect", {
                  attrs: {
                    x: x > 0 ? x - 128 -26 : x+106+26,
                    y: y+8 - boxHeight/2,
                    width:22,
                    height: 14,
                    stroke:"#FF9595",
                    fill:'#FF9595',
                    radius: 9,
                  },
                  draggable: true,
                });
                group.addShape('text',{
                  attrs:{
                    text:`1`,
                    fill:'white',
                    width:22,
                    height: 14,
                    x: x > 0 ? x - 128 -26 + 12  : x+106+26 +12,
                    y:y+16 - boxHeight/2,
                    textAlign:"center",
                    textBaseline:'middle',
                    fontSize: 10,
                    fontWeight:500,
                  },
                  name:`target-${value}`,
                })


                positions.push({x:x > 0 ? x - 128 -26 + 12-21  : x+106+26 +12+20,y:y+16 - boxHeight/2})
              }

              {
                console.log(positions.map((pos,i)=>[i===0 ? 'M':'L',pos.x , pos.y]));


                const startPoint = positions[0],
                    endPoint = positions[1];

                const startPoint2 = {
                      x:0,
                      y:startPoint.y,
                    },
                    endPoint2 = {
                      x:0 ,
                      y:endPoint.y,
                    };

                console.log(endPoint2.y)

                {
                  const _r = 345  ;
                  const xDiff = Math.sqrt(_r * _r - (startPoint2.y ) * (startPoint2.y ));
                  startPoint2.x = startPoint.x > 0 ?  xDiff : - xDiff;
                }

                {
                  const _r = 250  ;
                  const xDiff = Math.sqrt(_r * _r - (endPoint2.y ) * (endPoint2.y ));
                  endPoint2.x = endPoint.x > 0 ?  xDiff : - xDiff;
                }

                group.addShape("path", {
                  attrs: {
                    startArrow: {
                      // 自定义箭头指向(0, 0)，尾部朝向 x 轴正方向的 path
                      path: 'M 0,0 L 4,2 L 4,-2 Z',
                      // 箭头的偏移量，负值代表向 x 轴正方向移动
                      // d: -10,
                      fill:'#A955EA',
                    },
                    endArrow: {
                      // 自定义箭头指向(0, 0)，尾部朝向 x 轴正方向的 path
                      path: 'M 0,0 L 4,2 L 4,-2 Z',
                      // 箭头的偏移量，负值代表向 x 轴正方向移动
                      // d: -10,
                      fill:'#FF9595',
                    },
                    stroke: `l(0) 0:#A955EA30  1:#FF959510 `,
                    lineWidth: 2,
                    path: [
                      ["M", startPoint.x, startPoint.y],
                      ["L", startPoint2.x, startPoint.y],
                      // [
                      //   "C",
                      //   endPoint2.x*(10/9) / 3 + (2 / 3) * startPoint2.x*(9/10),
                      //   startPoint2.y,
                      //   endPoint2.x*(10/9) / 3 + (2 / 3) * startPoint2.x*(9/10),
                      //   endPoint2.y,
                      //   endPoint2.x*(10/9),
                      //   endPoint2.y,
                      // ],
                      ["L", endPoint2.x, endPoint2.y],
                      ["L", endPoint.x, endPoint.y],
                    ],
                    cursor:'pointer'
                  },
                  name: "path-shape",
                })
              }

            }


          })





        })
        const shape = group
        this.drawText(cfg,group)
        return shape
      },
      afterDraw(cfg, group){
        // const textShape = group.find(ele=> ele.get('name') === 'line-chart-label');
        // const  rate = 1.01
        // textShape.animate(ratio=>{
        //   const currentFontSize = textShape.attr('fontSize')||15
        //   const scale = ratio<0.5 ? rate : (1/rate)
        //   const targetFontSize = currentFontSize*scale;
        //   return {fontSize: targetFontSize}
        // },{
        //   duration:1000,
        //   repeat:true,
        // })
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

  registerNode();
  const data = {
    nodes:[
      {
        id:'node1',
        label:'Node 1',
        x:width/2,
        y:height/2,
        centerColor:"#bae7ff",
        details:[
          {cat:'pv',values :[
              10086,30,40,30,30,20,30,40,30,30,
              20,30,40,30,30,20,30,40,30,30,
              20,30,40,30,30,20,30,40,30,30,
            ] , color:'#5B8FF9'},
        ]
      },
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
    edges:[
      {
        source: "node1",
        sourceKey: "source-10086",
        target: "node2",
        targetKey:  "target-10086",
      }
    ]
  }
  const graph = new G6.Graph({
    // 控件
    container :container,
    // 宽度
    width:width,
    // 高度
    height:height,
    // 布局
    layout:{
      type:'dagre',
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
    defaultEdge: {
      type: 'dice-er-edge',
      style: {
        stroke: '#e11d1d',
        lineWidth: 4,
        endArrow: false,
      },
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

  // graph.on('center-img:mouseenter',e=>{
  //   const {target} = e;
  //   target.animate({
  //     shadowColor:'rgba(44,104,255,0.3500)',
  //     shadowBlur:10,
  //   },{
  //     duration:0,
  //     repeat:false
  //   })
  // })
  //
  // graph.on('center-img:mouseleave',e=>{
  //   const {target} = e;
  //   target.animate({
  //     shadowColor:'rgba(44,104,255,0.0500)',
  //     shadowBlur:10,
  //   },{
  //     duration:0,
  //     repeat:false
  //   })
  // })

  graph.on('item-content:mouseenter',e=>{
    const {target} = e;
    target.animate({
      shadowColor:'rgba(44,104,255,0.3500)',
      shadowBlur:10,
    },{
      duration:0,
      repeat:false
    })
  })

  graph.on('item-content:mouseleave',e=>{
    const {target} = e;
    target.animate({
      shadowColor:'rgba(44,104,255,0.0500)',
      shadowBlur:10,
    },{
      duration:0,
      repeat:false
    })
  })

  // graph.on('path-shape:mouseenter',e=>{
  //   const {target} = e;
  //   target.animate({
  //     stroke: `l(0) 0:#A955EA45  1:#FF959530 `,
  //     lineWidth: 3,
  //   },{
  //     duration:0,
  //     repeat:false
  //   })
  // })
  //
  // graph.on('path-shape:mouseleave',e=>{
  //   const {target} = e;
  //   target.animate({
  //     stroke: `l(0) 0:#A955EA30  1:#FF959515 `,
  //     lineWidth: 2,
  //   },{
  //     duration:0,
  //     repeat:false
  //   })
  // })


}

nextTick(()=>{
  initTree()
})

</script>

<style scoped>
#container{
  width: 100%;
  height: 800px;
  background: #F4F8FF;
}
</style>