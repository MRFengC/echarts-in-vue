// 条形图
<template>
  <div>
    <div v-if="ishavedata"
      :id="cid"
      :style="diskAnalysis" />
  </div>
</template>
<script>
const echarts = require('echarts');

export default {
  // 传参
  /* 多颜色条形图表
   **cid: 页面唯一id
   **cheight: 页面突变高度
   **ctitle: 图表标题
   **cdata: 图表数据[]  [320, 302, 301, 334, 390, 330, 320]
   ** cname:[]轴中文 ['周一','周二','周三','周四','周五','周六','周日']
   */
  props: {
    cid: Number,
    cheight: String,
    ctitle: String,
    cdata: Array,
    cname: Array,
  },
  data() {
    return {
      myChart: '',
      diskAnalysis: {
        width: '100%',
        height: this.cheight,
      },
      // 配置
      echarts1_option: {
        option: {
          title: {
            text: this.ctitle,
            left: '16px',
            top: '16px',
          },
          tooltip: {},
          legend: {
            data: [],
            itemWidth: 8, // 设置宽度
            padding: [-10, 0, 0, 0],
            itemGap: 28,
            itemHeight: 8, // 设置高度
            bottom: '0px',
            icon: 'circle',
            selectedMode: false, // 取消图例上的点击事件
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '5%',
            containLabel: true,
          },

          xAxis: {
            splitLine: {
              show: true,
              lineStyle: {
                type: 'dashed', // 设置背景为虚线
              },
            },
            type: 'value',
            // axisLine: {
            //   lineStyle: {
            //     color: 'rgba(0,0,0,0.25)',
            //   },
            // },
            axisLine: {
              // y轴
              show: false,
            },
            axisTick: {
              // y轴刻度线
              show: false,
            },
            axisLabel: {
              show: true,
              textStyle: {
                color: 'rgba(0,0,0,0.65)',
              },
            },
          },
          yAxis: {
            type: 'category',
            data: [],
            max: 1, // 最大刻度

            axisLine: {
              lineStyle: {
                color: 'rgba(0,0,0,0.25)',
              },
            },
            axisLabel: {
              show: true,
              textStyle: {
                color: 'rgba(0,0,0,0.65)',
              },
            },
          },
          series: [],
          color: ['#F5A623', '#13CE66', '#1890FF', '#546570', '#c4ccd3'],
        },
      },
    };
  },
  computed: {
    ishavedata() {
      if ((this.cheight !== '' && this.seriesData !== [] && this.cxaxis !== [], this.cclass !== [])) {
        return true;
      }
      return false;
    },
  },
  // 监听传值
  watch: {
    cname: {
      handler(value) {
        try {
          this.cname = value;
          this.ProcessingData();
          this.myChart.setOption(this.echarts1_option.option);
        } catch (err) {
          return err;
        }
      },
      deep: true,
    },
    cdata: {
      handler(value) {
        try {
          this.cdata = value;
          this.ProcessingData();
          this.myChart.setOption(this.echarts1_option.option);
        } catch (err) {
          return err;
        }
      },
      deep: true,
    },
  },
  created() {
    this.ProcessingData();
  },
  mounted() {
    try {
      const that = this;
      // 基于准备好的dom，初始化echarts实例
      this.myChart = echarts.init(document.getElementById(this.cid));
      // 绘制图表，this.echarts1_option是数据
      this.myChart.setOption(this.echarts1_option.option);
      window.addEventListener('resize', () => {
        that.myChart.clear();
        that.myChart.setOption(that.echarts1_option.option);
        that.myChart.resize();
      });
    } catch (err) {
      return err;
    }
  },
  methods: {
    /*
     ** 处理数据
     */
    ProcessingData() {
      this.echarts1_option.option.legend.data = [];
      this.echarts1_option.option.yAxis.data = [];

      this.echarts1_option.option.legend.data = this.cname;
      this.echarts1_option.option.yAxis.data = this.cname;
      const cdatalen = this.cdata.length;
      const cnamelen = this.cname.length;
      this.echarts1_option.option.series = [];
      if (cdatalen === cnamelen && cdatalen !== 0 && cnamelen !== 0) {
        for (let i = 0; i < cdatalen; i++) {
          const Arraydata = [];
          for (let a = 0; a <= i; a++) {
            if (a === i) {
              Arraydata.push(this.cdata[i]);
            } else {
              Arraydata.push('undefine');
            }
          }
          const cseries = {
            name: this.cname[i],
            type: 'bar',
            stack: '总量',

            barWidth: 28,
            barCategoryGap: '48%', // 间距
            label: {
              normal: {
                show: true,
                position: 'insideRight',
              },
            },
            data: Arraydata,
          };
          this.echarts1_option.option.series.push(cseries);
        }
      }
    },
  },
  Destroy() {
    this.myChart.clear();
  },
};
</script>
<style lang="less" scoped>

</style>
