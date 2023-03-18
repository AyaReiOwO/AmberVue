<template>
  <div id="graph"></div>
</template>

<script setup>
import G6 from '@antv/g6/dist/g6.min'

const registerNode = ()=>{
  G6.registerNode(
      'k-rect', // 自定义的节点名称
      {
        // 添加文本节点
        drawText(cfg, group) {
          group.addShape('text', {
            attrs: {
              x:            cfg.style.width/2,
              y:            cfg.style.height/2,
              text:         cfg.label,
              fill:         '#fff',
              fontSize:     14,
              textAlign:    'center',
              textBaseline: 'middle',
              ...cfg.labelCfg.style,
            },
            className: 'node-text',
            draggable: true,
          });
        },
        /*
          绘制节点, 包含文本, 锚点等
        */
        draw(cfg, group) {
          /*
          * group: 节点分组, 包含节点配置, 锚点, 图标, 文本等shape.
          * 绘制流程:
          * 1. 获取默认样式配置
          * 2. 添加 shape
          * 3. 添加文本节点/锚点等
          * 4. return shape
          */

          // 1.
          const attrs = cfg;

          // console.log(attrs);
          // 2.
          const shape = group.addShape(
              'rect', // 继承内置节点的 shape, 可选 rect, circle, ellipse, path 等
              {
                // 所有的样式配置
                attrs: {
                  ...attrs,
                  fill: '#1890ff',
                  ...attrs.style,
                },
                className: 'custom-shape', // 添加自定义属性, 方便以后对节点进行查找更新等
                draggable: true, // 允许自定义图形使用拖拽事件
              },
          );

          // 3.
          // this 是当前节点的实例, 并不是 Vue 实例
          this.drawText(cfg, group);

          // 4.
          return shape;
        },
        /* 绘制后附加操作 */
        afterDraw(cfg, group) {},
        /* 用于更新节点的配置 */
        update(cfg, node) {},
        /* 用于更新节点后的附加操作 */
        afterUpdate(cfg, node) {},
        /*
          设置节点状态, 主要是交互状态, 如 hover, active 等.
        */
        setState(name, value, group) {},
        /* 获取当前节点的锚点 */
        getAnchorPoints(cfg) {},
      },
      // 'extendNodeName',
  );

}

const initTree = (data)=> {
  registerNode();
  const graph = new G6.Graph({
    container: document.getElementById('graph'),
    width:     window.innerWidth - 40,
    height:    window.innerHeight - 40,
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