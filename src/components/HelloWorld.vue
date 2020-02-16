<template>
  <div class="hello">
  <!-- 初始化echarts需要一个有宽高的盒子 -->
  <div ref='mapbox' style='height:400px;width:600px'>

  </div>
  </div>
</template>

<script>
import echarts from 'echarts'
const option = {
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
};

export default {
  name: 'HelloWorld',
  mounted(){
    let mychart = echarts.init(this.$refs.mapbox)
    mychart.setOption(option)
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
