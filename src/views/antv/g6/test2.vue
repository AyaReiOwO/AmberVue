<template>
  <div id="container"></div>
</template>

<script setup>
import G6 from '@antv/g6/dist/g6.min'

const registerNode = ()=>{
    G6.registerNode('k-rect',{
      drawText(cfg,group){
        group.addShape('text',{
          attrs: {
            text:         cfg.label,
            fill:         '#fff',
            fontSize:     14,
            textAlign:    'center',
            textBaseline: 'middle',
            ...cfg.labelCfg.style,
          },
          className: 'node-text',
          draggable: true,
        })
      },
      draw(cfg,group){
        //1.
        const attrs = cfg;
        const shape = group.addShape(
            'rect',//继承内置节点的shape，可选rect,circle,ellipse,path,
            {
              attrs:{
                fill:'#1890ff',
                ...attrs,
                ...attrs.style,
              }
            })
        this.drawText(cfg,group)
        return shape
      },
      afterDraw(){

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
    layout:    {
      type: 'dagre',
    },
    defaultNode: {
      type:  'k-rect',
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


}

const data = {
  nodes: [
    {
      id:    '1',
      label: 'node 1',
    },
    {
      id:    '2',
      label: 'node 2',
    },
    {
      id:    '3',
      label: 'node 3',
    },
    {
      id:    '4',
      label: 'node 4',
    },
  ],
  edges: [
    {
      source: '1',
      target: '2',
    },
    {
      source: '1',
      target: '3',
    },
    {
      source: '2',
      target: '4',
    },
    {
      source: '3',
      target: '4',
    },
  ],
};
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