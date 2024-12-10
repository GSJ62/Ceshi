<template>
  <div class="test2">
    <h3>线路开通情况</h3>
    <div id="main_pie"></div>
    <div class="chart"></div>
  </div>
</template>

<script>

import * as echarts from 'echarts';
import { start } from 'nprogress';
export default {
  name: "pie",
  name: 'line',
  data() {
    return {
      //数据
      xy_data: [
        { name: '广州市', value: 21 },
        { name: '佛山市', value: 6 },
        { name: '深圳市', value: 17 },
        { name: '东莞市', value: 1 },
        //颜色渐变
        //color: new echarts.graphic.LinearGradient(0, 0, 0, 1,[{offset: 0, color: '#0856ff'},{offset: 1, color: '#c0d4fe'}])
        //color: new echarts.graphic.LinearGradient(0, 0, 0, 1,[{offset: 0, color: '#61a9fd'},{offset: 1, color: '#b3d5fe'}])
        //color: new echarts.graphic.LinearGradient(0, 0, 0, 1,[{offset: 0, color: '#63d286'},{offset: 1, color: '#a3c4ad'}])
        //color: new echarts.graphic.LinearGradient(0, 0, 0, 1,[{offset: 0, color: '#ffc630'},{offset: 1, color: '#ffe49c'}])
      ],
    };
  },
  created() {
    this.initPie();
  },
  methods: {
    initPie() {
      //圆环图
      this.$nextTick(() => {
        let myChart = echarts.init(document.getElementById('main_pie'));
        let option = {
          title: [
            {
              text: '总计',
              textStyle: {
                fontSize: '15',
                color: '#A9A9A9',
              },
              show: true,
              x: 'center',
              top: '40%',
              subtext: '45',
              subtextStyle: {
                color: '#000',
                fontSize: 30,
              },
            },
            {
              subtext: '单位： 条',
              top: '1%'
            },
          ],
          tooltip: {},
          legend: {
            //头部导航图形
            icon: 'circle',
            //设置图形的大小显示
            itemWidth: 8,
            itemHeight: 8,
            data: this.xy_data.map(item => {
              return {
                name: item.name,
              };
            })
          },
          series: [
            {
              name: '',
              value: '',
              //设置颜色渐变
              itemStyle: {
                normal: {
                  color: function (params) {
                    var colorList = [
                      ['#0856ff', '#c0d4fe'],
                      ['#61a9fd', '#b3d5fe'],
                      ['#63d286', '#a3c4ad'],
                      ['#ffc630', '#ffe49c'],
                    ];
                    var index = params.dataIndex;
                    return new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                      offset: 0,
                      color: colorList[index][0]
                    },
                    {
                      offset: 1,
                      color: colorList[index][1]
                    }
                    ])
                  },
                }
              },
              legendHoverLink: true,
              //外围内环圆角化
              radius: ['50%', '60%'],
              label: {
                show: true,
              },
              type: 'pie',
              data: this.xy_data.map(item => {
                return {
                  name: item.name,
                  value: item.value
                };
              })
            }
          ],
        };
        myChart.setOption(option);
      })
    },

    //折线图
    initChart() {
      const dom = document.querySelector(".chart");
      const chart = this.$echarts.init(dom);
      const option1 = {
        title: {
          subtext: '单位： 条',
          fontSize: '10',
        },
        tooltip: {
          trigger: "axis",
        },
        legend: {
          //水平方向显示
          orient: 'horizontal',
          x: 'center',
          y: 'top',
          itemWidth: 15,
          itemHeight: 0,
          data: ["佛山市", "东莞市", "深圳市", "广州市"],
        },
        grid: {
          left: "2%",
          right: "4%",
          bottom: "3%",
          containLabel: true,
        },
        //X轴
        xAxis: {
          //Y轴沿着X轴刻度上的线
          splitLine: {
            show: false
          },
          axisLine: {
            show: false
          },
          axisTick: {
            show: false,
            alignWithLabel: true
          },
          data: this.xAxis,
          boundaryGap: false,
          interval: 6,
          min: 0,
          max: 16,
          type: "category",
          gridIndex: 0,
          data: ["1997", "2002", "2004", "2005", "2009", "2010", "2011", "2013", "2014", "2016", "2017", "2018", "2019", "2020", "2021", "2022", "2024",],
        },
        //Y轴
        yAxis: {
          min: 0,
          max: 5,
          interval: 1,
          type: 'value',
          gridIndex: 0,
          splitLine: {
            show: true
          },
          //Y轴有无实线显示
          axisLine: {
            show: false
          },
          //Y轴刻度条有无显示
          axisTick: {
            show: false,
            alignWithLabel: true
          }
        },
        series: [
          {
            name: "佛山市",
            type: "line",
            symbol: 'none',
            smooth: false,
            itemStyle: {
              normal: {
                color: '#0e7ce2',
                lineStyle: {
                  color: '#0e7ce2'
                }
              }
            },
            data: [0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 0, 2, 1, 0],
          },
          {
            name: "东莞市",
            type: "line",
            symbol: 'none',
            smooth: false,
            itemStyle: {
              normal: {
                color: '#ff8352',
                lineStyle: {
                  color: '#ff8352'
                }
              }
            },
            data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0],
          },
          {
            name: "深圳市",
            type: "line",
            symbol: 'none',
            smooth: false,
            itemStyle: {
              normal: {
                color: '#e271de',
                lineStyle: {
                  color: '#e271de'
                }
              }
            },
            data: [0, 0, 2, 0, 0, 2, 1, 0, 0, 3, 1, 0, 0, 4, 1, 3, 0],
          },
          {
            name: "广州市",
            type: "line",
            symbol: 'none',
            smooth: false,
            itemStyle: {
              normal: {
                color: '#f8456b',
                lineStyle: {
                  color: '#f8456b'
                }
              }
            },
            data: [1, 1, 0, 2, 1, 3, 0, 1, 1, 1, 3, 2, 0, 1, 1, 1, 1],
          }
        ],
        dataZoom: [
          {
            type: 'slider',
            show: true,
            startValue: 14,
            endValue:19,
            xAxisIndex: [0],
            bottom: -18,
          },
          {
            type: 'inside',
            start: 0,
            end: 10,
            xAxisIndex: [0]
          }
        ]
      };
      chart.setOption(option1);

    },

  },
  mounted() {
    this.initChart();

    this.registerUser()
  },
};
</script>

<style>
.test2 {
  background-color: white;
  width: 100%;
  height: 100%;
  margin: 5px;
}

h3 {
  background-color: #eaf1ff;
  width: 100%;
  height: 60px;
  line-height: 60px;
  border-radius: 10px;
  padding-left: 10px;
}

#main_pie {
  width: 420px;
  height: 400px;
  float: left;
  margin-top: 20px;
  margin-left: 50px;
}

.chart{
  width: 550px;
    height: 350px;
    margin-top: 20px;
    margin-left: 600px;
}
</style>
