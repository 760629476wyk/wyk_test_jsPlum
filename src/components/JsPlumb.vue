<template>
  <div id="jsPlumbPaint" class="jsPlumb">
    <div class="source" id="source"></div>
    <div class="source1" id="source1"></div>
    <div class="target" id="target"></div>
  </div>
</template>

<script>

import {jsPlumb} from 'jsplumb'

export default {
  name: 'plumb',
  data () {
    return {}
  },
  mounted () {
    let operation = {
      Anchor: 'BottomCenter', // 端点的定位点的位置声明（锚点）：left，top，bottom等
      Anchors: [null, null], // 多个锚点的位置声明
      ConnectionsDetachable: false, // 连接是否可以使用鼠标默认分离
      ConnectionOverlays: [], // 附加到每个连接的默认重叠
      Connector: 'Bezier', // 要使用的默认连接器的类型：折线，流程等
      Container: null, // 设置父级的元素，一个容器
      DoNotThrowErrors: false, // 如果请求不存在的Anchor，Endpoint或Connector，是否会抛出
      DragOptions: {}, // 用于配置拖拽元素的参数
      DropOptions: {}, // 用于配置元素的drop行为的参数
      Endpoint: 'Dot', // 端点（锚点）的样式声明（Dot）
      Endpoints: [null, null], // 多个端点的样式声明（Dot）
      EndpointOverlays: [], // 端点的重叠
      EndpointStyle: {fill: '#456'}, // 端点的css样式声明
      EndpointStyles: [null, null], // 同上
      EndpointHoverStyle: null, // 鼠标经过样式
      EndpointHoverStyles: [null, null], // 同上
      HoverPaintStyle: null, // 鼠标经过线的样式
      LabelStyle: {color: 'black'}, // 标签的默认样式。
      LogEnabled: false, // 是否打开jsPlumb的内部日志记录
      Overlays: [], // 重叠
      MaxConnections: 1, // 最大连接数
      PaintStyle: {lineWidth: 8, stroke: '#456'}, // 连线样式
      ReattachConnections: false, // 是否重新连接使用鼠标分离的线
      RenderMode: 'svg', // 默认渲染模式
      Scope: 'jsPlumb_DefaultScope'// 范围，标识
    }
    console.log(operation)
    let instance, instance1
    jsPlumb.bind('ready', function () {
      // 设置父容器
      jsPlumb.setContainer('jsPlumbPaint')
      // 设置实例的参数
      let parameter = {
        PaintStyle: {
          strokeWidth: 10, // 连线的宽度
          stroke: '#567567', // 连线的颜色
          outlineStroke: '#ff94d5', // 连线外层边的颜色
          outlineWidth: 30 // 连线外边的宽度
        },
        ReattachConnections: true, // true ，用鼠标断开链接后再次自动连接
        ConnectionsDetachable: true, // 是否用鼠标拖动可以断开连线
        Connector: 'Straight', // 连线的样式是直线，弯曲程度
        /**
         * 类型：
         *  straight：直线
         *    stub -可选，默认为0。此参数的任何正值将导致该长度的存根从直线段连接到连接的另一端之前的端点发出。
         *    gap -可选，默认为0。端点与存根的起点或连接到另一个端点的段之间的间隔。
         *  bezier：贝塞尔曲线（默认）
         *    curviness- 可选的; 默认值为150。这定义了Bezier控制点到锚点的距离（以像素为单位）
         *  flowachart：流程图
         *    stub-这是从端点发出的初始存根的最小长度（以像素为单位）。这是一个可选参数，可以是一个整数，该整数指定连接器两端的存根，也可以是两个整数的数组，指定连接中[source，target]端点的存根。默认值为30像素的整数。
         *    alwaysRespectStubs-可选，默认为false。此参数指示jsPlumb始终在每个端点之外绘制指定桩号长度的行，而不是在两个元素比两个桩号之和更近时自动减少桩号。
         *    gap-可选，默认为0像素。使您可以指定连接器末端与连接它的元素之间的间隙。
         *    midpoint-可选，默认为0.5。这是将绘制流程图连接器最长部分的两个元素之间的距离。此参数在您具有以编程方式控制图形并且可能希望避免页面上其他元素的情况下很有用。
         *    cornerRadius 默认值为0。此参数为正值将导致弯曲的角段。
         *  state machine：（状态机）
         *    margin- 可选的; 默认值为5。定义到连接器开始/结束的元素的距离。
         *    curviness -可选，默认为10。这与贝塞尔曲线上的“曲线”参数具有相似的效果。
         *    proximityLimit -可选，默认为80。连接器将自身绘制为直线而不是二次贝塞尔曲线之前，其两端之间的最小距离。就是小于80 都是直线
         * */
        Endpoint: ['Dot', {radius: 15}], // 端点是长方形，角圆弧程度5
        /**
         * Dot：实心点
         *    radius- 可选的; 默认为10像素。定义点的半径。
         *    cssClass- 可选的。附加到Endpoint创建的元素的CSS类。
         *    hoverClass- 可选的。每当鼠标悬停在元素或附加的Connection上时，便会在端点创建的元素上附加一个CSS类。
         * Rectangle：长方形
         *    width可选的; 默认为20像素。定义矩形的宽度。
         *    height可选的; 默认为20像素。定义矩形的高度。
         *    cssClass可选的。附加到Endpoint创建的元素的CSS类。
         *    hoverClass可选的。每当鼠标悬停在元素或附加的Connection上时，便会在端点创建的元素上附加一个CSS类。
         * Blank
         *    不绘制用户可见的任何内容。
       *      如果您需要用户能够拖动现有的Connections，则可能不是您想要的该Endpoint-为此，请使用Rectangle或Dot Endpoint并为其分配一个CSS类，以使其透明。
         * Image
         *    src需要。指定要使用的图像的URL
         *    cssClass可选的。附加到Endpoint创建的元素的CSS类。
         *    hoverClass可选的。每当鼠标悬停在元素或附加的Connection上时，便会在端点创建的元素上附加一个CSS类。
         * */
        EndpointStyle: {fill: '#f95a2c'}, // 端点颜色
        EndpointHoverStyle: {fill: '#3bff4c'}, // 选中时端点样式
        Anchor: ['TopRight', 'Right', 'BottomRight', 'Bottom', 'BottomLeft', 'Left', 'TopLeft'], // 设置锚点样式
        /**
         * 类型：
         *  静态:
         *    单值：Top TopRight Right BottomRight Bottom BottomLeft Left TopLeft Center
         *          上  右上     右    右下        下     左下       左   左上    中间
         *    多值：[x,y,dx,dy,offsetX,offsetY]
         *          左上角是（0，0）0.5是居中
         *        x:横坐标，取值范围0-1
         *        y:横坐标，取值范围0-1
         *        dx:锚点向横轴射出线，有0，-1，1三个值来设置。0为不放射线。
         *        dy:锚点向纵轴射出线，有0，-1，1三个值来设置。0为不放射线。
         *        基于元素中心（x 轴朝右，y 轴朝下），(0, -1) 向上，(0, 1) 向下，(1, 0) 向右，(-1, 0) 向左，（0, 0） 任意，不传就是 (0, 0)。
         *        offsetX:锚点偏移量x（px）
         *        offsetY:锚点偏移量y（px）
         *  动态：['Top', 'TopRight', 'Right', 'BottomRight', 'Bottom', 'BottomLeft', 'Left', 'TopLeft']
         *        根据环境自己选择合适位置，数组中给出的位置
         *  周边锚： ['Perimeter', { shape: 'Triangle', anchorCount: 150 }]
         *      样式：端点在source中的活动路线
         *      Circle：圆
         *      Ellipse：椭圆
         *      Triangle：三角形
         *      Diamond：菱形
         *      Rectangle：长方形
         *      Square：正方形
         *      可以自定义星钻个
         *      anchorCount: 150 点数越多，操作越流畅。而且浏览器还需要做更多的工作。
         *  连续锚：anchor: [ 'Continuous', { faces: [ 'top' ] } ]
         *      根据位置自动计算断点位置，faces设置锚点的位置固定在上面
         *
         *  耗能：连续锚 >  周边 = 动态 > 静态
         *  样式和锚点关联
         *  添加前缀jtk-endpoint-anchor-
         *  例如：var ep = jsPlumb.addEndpoint("someDiv", {
         *    anchor:[[ 0.5, 0, 0, -1, 0, 0, "foo bar" ],
         *    [ 0.5, 0, 0, -1, 0, 0, "top" ],
         *    [ 1, 0.5, 1, 0, 0, 0, "right" ]
         *    [ 0.5, 1, 0, 1, 0, 0, "bottom" ]]
         *  });
         *  生成类：jtk-endpoint-anchor-foo 和 bar jtk-endpoint-anchor-top jtk-endpoint-anchor-right jtk-endpoint-anchor-bottom
         * */
        Overlays: [['Arrow', {location: 0.7, direction: -1, foldback: 0.001, width: 70, length: 50}]]// 重叠
        /***
         * 叠加层
         *    Arrow 箭头 -绘制一个箭头，使用四个点：头部和两个尾点，以及一个foldback允许箭头尾部缩进的点。
         *      width 宽度 -箭头尾部的宽度
         *      length 长度 -从箭头的尾部到头部的距离
         *      location 位置 -位置，无论是从0到1（包括0到1）的比例值，还是绝对值（负值表示距目标的距离；正值大于1表示距源的距离），箭头应出现在连接器上
         *      direction 方向 -指向哪种方式。允许值为1（默认值，表示向前）和-1（表示向后）
         *      foldback  折返 -沿着箭头的轴线指向尾部折返的距离。默认值为0.623。
         *      paintStyle 绘画风格-Endpoints 和 Connectors 的样式对象。
         *    Label 标签 -可配置的标签，沿着连接器的某个位置绘制。
         *      label 标签 -要显示的文本。您可以在此处提供一个函数，而不是纯文本：它是作为参数传递给Connection的，它应该返回一个String。
         *      cssClass-用于Label的可选css类。现在，这比使用labelStyle参数更可取。
         *      labelStyle-标签外观的可选参数。此JS对象中的有效条目是：
         *        font-适用于Canvas元素的格式的字体字符串
         *        fill-用来填充标签背景的颜色。可选的。
         *        color-标签文本的颜色。可选的。
         *        padding-标签的可选填充。这表示为标签宽度的比例，而不是像素或ems。
         *        borderWidth-标签边框的可选宽度（以像素为单位）。预设为0。
         *        borderStyle-可选。绘制边框的颜色（如果有）。
         *      visible: Boolean 是否可见，默认 true
         *      location  位置 -至于箭头叠加层。标签应出现在从0到1的比例范围内（包括0到1），或者作为与源或目标的绝对偏移量。
         *        getLocation：返回当前位置
         *        setLocation：设置当前位置，对于端点，位置以[x，y]数组表示，其值与端点的宽度/高度成正比（0-1（包括0-1）的小数）或绝对值（小数大于0）
         *        (1) 指定一个覆盖在端点的中心位置：
         *          location:[ 0.5, 0.5 ]
         *        (2) 从左上角沿x轴叠加5像素
         *          location: [ 5, 0 ]
         *        (3) 从右下角沿x轴叠加5像素
         *          location: [ -5, 0 ]
         *    PlainArrow 箭头形状的三角形 -箭头形状的三角形，没有折返。foldback=1
         *    Diamond  菱形 -顾名思义就是菱形。foldback=2
         *    Custom  自定义 -允许您自己创建叠加层-叠加层可以是您喜欢的任何DOM元素。
         *  所有叠加层都支持以下两种获取/设置位置的方法：
         *    getLocation-返回当前位置
         *    setLocation-设置当前位置。对于端点，位置以[x，y]数组表示，其值与端点的宽度/高度成比例（小数在0-1范围内（包括0-1））或绝对值（小数大于0）。
         */
      }
      // 实例化jsplumb
      instance = jsPlumb.getInstance(parameter)
    })
    // 设置两个元素之间的连线
    instance.connect({
      source: 'source',
      target: 'target',
      scope: 'someScope'
    })
    // 设置拖拽
    instance.draggable('source')
    instance.draggable('source1')
    instance.draggable('target')
    // 添加一个锚点
    instance.addEndpoint('source1', {
      anchor: 'Center',
      paintStyle: { full: '#0f53d9' }
    })
    // 创建另一个实例
    instance1 = jsPlumb.getInstance(operation)
    // 添加连线
    /**
     * 暂停绘图
     */
    instance1.setSuspendDrawing(true)
    instance.connect({
      source: 'source1',
      target: 'target'
    })
    instance1.setSuspendDrawing(false, true)
    /**
     * 批量暂停绘图
     */
    instance1.batch(function () {
      // import here
    }, true)
    /**
     * 删除元素
     */
    instance1.remove('tempElement')
    instance1.empty('element')// 其下面的的元素被清空
    /**
     * 断开连接
     *  删除端点：
     *    未配置：     deleteEndpointsOnDetach:false
     *  不删除端点：
     *    配置：     deleteEndpointsOnDetach:false
     */
    let conn = instance1.connect({
      source: '1',
      target: '2'
    })
    instance1.detach(conn)
    /**
     * 通过删除元素删除其所有的元素
     *    el可能是：
     *      代表元素ID的字符串
     *      DOM元素
     *      代表单个元素的选择器
     *    参数是可选的，可能包含：
     *      fireEvent-是否触发断开连接事件。默认值为true。
     */
    instance1.deleteConnectionsForElement('el', ['params'])
    /**
     * 从单个元素删除所有的链接
     */
    instance1.detachEveryConnection()
    /**
     * 添加单个端点
     */
    let endPoint3 = instance1.addEndpoint('3')
    /**
     * 删除单个端点
     */
    instance1.deleteEndPoint(endPoint3)
    /**
     * 删除所有端点
     */
    instance1.deleteEveryEndpoint()
    /**
     * 设置目标节点
     */
    instance1.makeTarget('foo', {
      deleteEndpointsOnDetach: false, // 删除节点的所有端点就会删除该target
      allowLoopback: false// 防止回环
    })
    /***
     * 位置查找器
     *  参数设置：
     *    eventOffset-释放鼠标按钮的左/上页面（一个JS对象）
     *    elementOffset -JS偏移对象，包含要在其上创建Connection的元素的偏移
     *    elementSize-要在其上创建Connection的元素的尺寸的[width，height]数组
     *    constructorParams-传递给Anchor的构造函数的参数。在上面给出的示例中，这些参数是“ position”和“ grid”；您可以传递任意参数。
     */
    let getLocation = function (eventOffset, elementOffset, elementSize, constructorParams) {
      let dx = eventOffset.left - elementOffset.left
      let dy = eventOffset.top - elementOffset.top
      let gx = elementSize[0] / (constructorParams.grid[0])
      let gy = elementSize[1] / (constructorParams.grid[1])
      let mx = Math.floor(dx / gx)
      let my = Math.floor(dy / gy)
      return [ ((mx * gx) + (gx / 2)) / elementSize[0], ((my * gy) + (gy / 2)) / elementSize[1] ]
    }
    console.log(getLocation)// 此函数的返回值是[x，y]的数组-介于0和1之间（含0和1）的比例值，例如可以传递给静态锚点。
    instance1.makeTarget('someElement', {
      anchor: [ 'Assign', {
        position: 'MyFinder',
        myCustomParameter: 'foo',
        anInteger: 5
      }]
    })
  }
}
</script>
<style scoped>
  .jsPlumb {
    background-color: #d2d2d2;
    height: 850px;
    width: 1500px;
    position: absolute;
  }

  .source {
    position: absolute;
    background-color: #42b983;
    width: 200px;
    height: 100px;
  }
  .source1 {
    position: absolute;
    background-color: #b9b037;
    left: 500px;
    width: 200px;
    height: 100px;
  }

  .target {
    position: absolute;
    background-color: #34c4ed;
    top: 500px;
    left: 250px;
    width: 200px;
    height: 100px;
  }
</style>
