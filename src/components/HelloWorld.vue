<template>
  <div class="hello">
  <!-- 初始化echarts需要一个有宽高的盒子 -->
  <div ref='mapbox' style='height:400px;width:700px'>

  </div>
  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/map/js/china'
const option1 = {
    xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    },
    yAxis: {
        type: 'value'
    },
    series: [{
        data: [820, 932, 901, 934, 1290, 1330, 1320],
        type: 'line'
    }]
}
// 使用地图先注册地图
const option2 = {
  title:{text:'疫情地图',link:'http://tangdd.cn',subtext:'全国疫情地图一览表'},
  series:[{
    data:[],
    type:'map',//控制是折线图还是地图
    map:'china',//控制是那个地区地图
    label:{ show:true ,color: 'red',fontSize:10},//控制对应地区的汉字
    itemStyle:{
      areaColor:'pink',
      borderColor:'blue'
    },//控制地图的颜色还有边框
    emphasis:{
      label:{
        color:'black',
        fontSize:12
      },
      itemStyle:{
        areaColor:'#ccc'
      }
    },//控制鼠标滑过之后背景色和字体颜色
    zoom:1.2,//控制地图的放大缩小
  }],
  //分层次显示地图颜色用下面这个东西,就是地图左下角那个东西
  visualMap:[{
    type:'piecewise',//条状
    show:true,
    splitNumber:5,//默认分为几条，就是看你要搞几个间断
    pieces:[
      {min:10000},
      {min:1000,max:9999},
      {min:100,max:999},
      {min:10,max:99},
      {min:1,max:9}
      ],
      align:'right',//控制字和条的位置，默认放到左边
      // orient:'horizontal',//控制整块的位置是平铺一大长条还是垂直啥的,默认垂直
      // left:40 ,//控制分段位置控制整块的位置
      // right:100 //控制分段位置控制整块的位置
      // showLabel:false,//这个没什么用处，会隐藏字
      // textStyle：{},//这个很明显是搞字体的
      inRange:{
        symbol:'rect',
        color:['red','white']
      },//这个控制小图是圆的还是方的啥的还有渐变色
      itemWidth:20,
      itemHeight:10
  }]
}
export default {
  name: 'HelloWorld',
  mounted(){
    let mychart = echarts.init(this.$refs.mapbox)
    // mychart.setOption(option1)
    mychart.setOption(option2)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
