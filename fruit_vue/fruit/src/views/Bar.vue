<template>
  <div id="myChart" :style="{width: '800px', height: '600px'}"></div>
</template>
<script>
  export default {
    name: 'Bar',
    mounted(){
      let _this = this
      axios.get('http://localhost:8181/fruit/barVO').then(function (response) {
        // 基于准备好的dom，初始化echarts实例
        let myChart = _this.$echarts.init(document.getElementById('myChart'))
        // 绘制图表
        myChart.setOption({
          title: {
            text: '水果销量统计',
            left: 'center',
            top: 20,
            textStyle: {
              color: '#555555'
            }
          },
          tooltip: {},
          xAxis: {
            data: response.data.names
          },
          yAxis: {},
          series: [{
            name: '销量',
            type: 'bar',
            data: response.data.values
          }]
        });
      })

    }
  }
</script>