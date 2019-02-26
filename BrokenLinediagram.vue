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
  /* 折线图
  **id: 唯一id
  ** ctitle:string    // 标题
  ** cAxis:['string', 'string'....] // 名称
  ** cheight:number px //高度
  seriesData: [{value:number,name: string},{value:number,name: string}...]   数值
  */
  props: { cid: Number, cheight: String, seriesdata: Array, cxaxis: Array, cclass: Array },
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
          tooltip: {
            // trigger: 'axis',
            // axisPointer: {
            //   type: 'cross',
            //   label: {
            //     backgroundColor: '#6a7985',
            //   },
            // },
            show: true,
          },
          grid: {
            y: '14px',
          },
          legend: {
            data: this.cclass,
            bottom: 0,

            itemWidth: 8, // 设置宽度

            itemHeight: 8, // 设置高度
            color: [
              '#1890FF',
              '#2FC25B',
              '#F5A623',
              '#F3A43B',
              '#60C0DD',
              '#D7504B',
              '#C6E579',
              '#F4E001',
              '#F0805A',
              '#26C0C0',
              '#F5A623',
              '#FF4949',
              '#546570',
              '#c4ccd3',
            ],
            left: 60,
            icon: 'circle',
            selectedMode: false, // 取消图例上的点击事件
          },
          xAxis: {
            type: 'category',
            boundaryGap: false, // 贴边
            data: this.cxaxis,
            nameTextStyle: {
              align: 'center',
            },
            axisLabel: {
              // interval: 0,
              // rotate: -0.5,
              // margin: 14,

              textStyle: {
                color: '#222',
              },
              formatter(value) {
                return value.length > 6 ? `${value.slice(0, 6)}...` : value;
              },
            },
            axisLine: {
              lineStyle: {
                color: 'rgba(0,0,0,0.25)',
              },
            },
            triggerEvent: true,
          },
          yAxis: {
            type: 'value',
            show: true,
            axisLine: {
              // y轴
              show: false,
            },
            axisTick: {
              // y轴刻度线
              show: false,
            },
            axisLabel: {
              textStyle: {
                color: 'rgba(0,0,0,0.65)',
              },
            },
            // axisLine: {
            //   lineStyle: {
            //     color: 'rgba(0,0,0,0.25)',
            //   },
            // },
          },
          series: [],
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
  watch: {
    seriesdata: {
      handler(value) {
        console.log(value);
        try {
          this.ProcessingData(value);
          this.myChart.setOption(this.echarts1_option.option);
        } catch (error) {
          if (error) {
            return error;
          }
        }
      },
      deep: true,
    },
    cxaxis: {
      handler(value) {
        // this.cxaxis = value;
        try {
          this.echarts1_option.option.xAxis.data = [];
          [...this.echarts1_option.option.xAxis.data] = value;
          this.myChart.setOption(this.echarts1_option.option);
        } catch (error) {
          if (error) {
            return error;
          }
        }
      },
      deep: true,
    },
    cclass: {
      handler(value) {
        try {
          this.echarts1_option.option.legend.data = [];
          [...this.echarts1_option.option.legend.data] = value;
          this.myChart.setOption(this.echarts1_option.option);
        } catch (error) {
          if (error) {
            return error;
          }
        }
      },
      deep: true,
    },
  },
  mounted() {
    // 基于准备好的dom，初始化echarts实例
    try {
      const that = this;
      if (this.ishavedata) {
        this.myChart = echarts.init(document.getElementById(this.cid));

        this.ProcessingData(this.seriesdata);
        // 绘制图表，this.echarts1_option是数据
        this.myChart.setOption(this.echarts1_option.option);
        // window.onresize = () => {
        //   this.$echarts.init(document.getElementById(this.cid)).resize();
        // };
        window.addEventListener('resize', () => {
          that.myChart.clear();
          that.myChart.setOption(that.echarts1_option.option);
          that.myChart.resize();
        });
      }
    } catch (e) {
      if (e) {
        return e;
      }
    }
  },
  created() {
    this.id = `myChart${this.cid}`;
  },
  methods: {
    // 设置相关参数
    ProcessingData(val) {
      this.echarts1_option.option.series = [];
      if (val !== [] && this.cclass !== []) {
        val.forEach((element, index) => {
          const color = [
            '#1890FF',
            '#2FC25B',
            '#F5A623',
            '#F3A43B',
            '#60C0DD',
            '#D7504B',
            '#C6E579',
            '#F4E001',
            '#F0805A',
            '#26C0C0',
            '#F5A623',
            '#FF4949',
            '#546570',
            '#c4ccd3',
          ];
          this.echarts1_option.option.series.push({
            name: this.cclass[index],
            data: element,
            type: 'line',
            // areaStyle: {},
            itemStyle: {
              normal: {
                color: color[index],
                lineStyle: {
                  color: color[index],
                },
                // color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                //   { offset: 0, color: color[index] },
                //   { offset: 1, color: '#fff' },
                // ]),
              },
            },
          });
        });
      }
    },
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
  margin-top: 100px;
  top: 50%;
}
.nodataimage img {
  width: 100%;
  vertical-align: middle;
}
</style>
