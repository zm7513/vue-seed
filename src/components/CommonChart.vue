<template>
  <div class="chartWrap">
    <!-- 这里是要渲染的图表 -->
    <div :id="chartId" style="height:360px;"></div>
  </div>
</template>
<script>
// 引入基本模板
let echarts = require("echarts/lib/echarts");
// 引入折线图组件
require("echarts/lib/chart/line");
require("echarts/lib/chart/bar");
// 引入提示框和图例组件
require("echarts/lib/component/title");
require("echarts/lib/component/tooltip");
require("echarts/lib/component/toolbox");
require("echarts/lib/component/legend");

export default {
  name: "commonchart",
  props: {
    chartData: {
      type: Object,
      required: true,
      default() {
        return {};
      }
    }
  },
  data() {
    return {
      myChartData: this.chartData
    };
  },
  computed: {
    // 生成随机字符，保证每个图表实例的ID都是唯一的
    chartId() {
      return Math.random()
        .toString(36)
        .substr(2);
    },
    // 根据唯一的ID，初始化一个图表实例
    myChart() {
      return echarts.init(document.getElementById(this.chartId));
    }
  },
  mounted() {
    // 首先需要先渲染一遍图表
    this.renderChart();
    // 监听窗口变化，一旦变化，立刻调用echarts的API，进行重新渲染
    window.addEventListener("resize", e => {
      this.myChart.resize();
    });
  },
  methods: {
    renderChart() {
      // 每次渲染前，得先清除掉上一次渲染的配置数据
      this.myChart.clear();
      this.myChart.setOption(this.myChartData);
    }
  },
  watch: {
    // 深度监听传入的数据变化，一定加deep属性哦
    chartData: {
      handler(v, o) {
        this.myChartData = v;
        this.renderChart();
      },
      deep: true
    }
  }
};
</script>
<style lang="scss">
#myChartWrap {
  width: 100%;
  height: 55vh;
}
</style>
