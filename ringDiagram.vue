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
  /* id: 唯一id
  ** ctitle:string    // 标题
  ** legendData:['string', 'string'....] // 图例
  ** cheight:number px //高度
  seriesData: [{value:number,name: string},{value:number,name: string}...]   数值
  */
  // props: ['cid', 'cheight', 'ctitle', 'legendData', 'seriesData'],
  props: { seriesData: Array, initpid: String, cid: Number, cheight: String, ctitle: String, legendData: Array },
  data() {
    return {
      diskAnalysis: {
        width: '100%',
        height: this.cheight,
      },
      id: this.cid,
      myChart: '',
      dataOption: {},
      GeneralAnalysisDataC: {},
      SubjectiveRatio: '',
      ObjectiveRatio: '',
      // 配置
      echarts1_option: {
        option: {
          title: {
            text: this.ctitle,
            left: '16px',
            top: '16px',
          },
          tooltip: {
            trigger: 'item',
          },
          legend: {
            orient: 'vertical',
            icon: 'circle',
            right: 24,
            itemWidth: 8, // 设置宽度

            itemHeight: 8, // 设置高度
            bottom: 0,
            data: this.legendData,
            selectedMode: false, // 取消图例上的点击事件
          },
          graphic: {
            type: 'text',
            left: 'center',
            top: 'center',
            style: {
              text: '圆环',
              textAlign: 'center',
              fill: '#4A4A4A',
              width: 30,
              height: 30,
              fontSize: 18,
            },
          },
          series: [
            {
              name: '',
              type: 'pie',
              radius: ['40%', '60%'],
              avoidLabelOverlap: false,
              label: {
                formatter: '{b|{b}}\n{per|{c}%}  ',
                // shadowBlur:3,
                // shadowOffsetX: 2,
                // shadowOffsetY: 2,
                // shadowColor: '#999',
                // padding: [0, 7],
                rich: {
                  a: {
                    color: '#999',
                    lineHeight: 22,
                    align: 'center',
                  },
                  b: {
                    fontSize: 16,
                    lineHeight: 33,
                  },
                  per: {
                    color: '#000',
                    fontWeight: 'bold',
                    padding: [2, 4],
                    borderRadius: 2,
                  },
                },
              },
              labelLine: {
                normal: {
                  show: true,
                },
              },
              data: this.seriesData,
            },
          ],
          color: ['#1890FF', '#2FC25B', '#13C2C2', '#F3647C', '#c4ccd3'],
        },
      },
    };
  },
  computed: {
    ishavedata() {
      if ((this.cheight !== '' && this.seriesData !== [] && this.cxaxis !== [], this.legendData !== [])) {
        return true;
      }
      return false;
    },
  },
  watch: {
    seriesData: {
      handler(value) {
        try {
          this.echarts1_option.option.series[0].data = value;
          this.myChart.setOption(this.echarts1_option.option);
        } catch (err) {
          return err;
        }
      },
      deep: true,
    },
    legendData: {
      handler(value) {
        try {
          this.echarts1_option.option.legend.data = value;
          this.myChart.setOption(this.echarts1_option.option);
        } catch (err) {
          return err;
        }
      },
      deep: true,
    },
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
  created() {
    this.id = `myChart${this.cid}`;
  },
  methods: {
    // 设置相关参数
  },
  Destroy() {
    this.myChart.clear();
  },
};
</script>
<style lang="less" scoped>
.titleName1 {
  position: absolute;
  top: 48px;
  font-size: 15px;
}
.nodataimage {
  margin-top: 30px;
  width: 200px;
  height: 200px;
  position: relative;
  left: 50%;
  margin-left: -100px;
  margin-top: 150px;
  top: 50%;
}
.nodataimage img {
  width: 100%;
  vertical-align: middle;
}
</style>
