<template>
  <div class="test2">
      <div class="header">
          <b>热点分析</b>
          <!-- 下拉框 -->
          <a-select class="select" default-value="近1个月" @change="handleChange">
              <a-select-option value="近1个月">近1个月</a-select-option>
              <a-select-option value="近2个月">近2个月</a-select-option>
              <a-select-option value="近3个月">近3个月</a-select-option>
              <a-select-option value="近半年">近半年</a-select-option>
              <a-select-option value="近1年">近1年</a-select-option>
          </a-select>
      </div>

      <div style="display: flex;">
          <!-- 圆环图1 -->
          <div id="main_pie" class="pie"></div>

          <!-- 热点 -->
          <div class="num" style="background-color: #f0f2f5;height: 400px;margin-right: 5px;">
              <div style="width: 690px;height: 80px;display: flex;background-color: #ffffff;margin-top: 5px;margin-bottom: 10px;">
                  <div class="sum-item">
                      <div><img src="/track/web/static/img/total.a4dae680.svg" class="alt"></div>
                      <div>
                          <div class="riaghttop">54</div>
                          <div class="rightbottom">热点总量</div>
                      </div>
                  </div>
                  <div class="sum-item">
                      <div><img src="/track/web/static/img/zm.82c41c04.svg" class="alt"></div>
                      <div>
                          <div class="riaghttop">38</div>
                          <div class="rightbottom">非消极热点数</div>
                      </div>
                  </div>
                  <div class="sum-item">
                      <div><img src="/track/web/static/img/fm.419c4906.svg" class="alt"></div>
                      <div>
                          <div class="riaghttop">16</div>
                          <div class="rightbottom">消极热点数</div>
                      </div>
                  </div>
              </div>

              <!-- 热点词云图例 -->
              <div id="container" style="width: 690px;height: 310px;background-color: #ffffff;">
              </div>
          </div>

          <!-- Top -->
          <div class="pie margin">
              <a-table :columns="columns" :data-source="datatext" :customHeaderRow="customHeaderRow"
                  :pagination="false" size="small" >
                  <template #title><b style="font-size: 16px;font-weight: 600;color: #000;">正面关键词top10</b></template>
              </a-table>
          </div>
      </div>

      <div style="display: flex;">
          <!-- 圆环图2 -->
          <div class="pie" id="main2"></div>

          <!-- tabs -->
          <div class="num" style="margin-top: 10px; margin-right: 5px;">
              <b style="font-size: 16px;font-weight: 600;color: #000;padding-top: 15px;padding-left: 15px;">热点事件</b>
              <a-tabel :data="dataAll">
                  <a-tabs default-active-key="1" @change="callback">
                      <a-tab-pane key="1" tab="全部">
                          <div class="list-item">
                              <a class="content-box" v-for="user in list">
                                  <a class="content">{{ user.text }}</a>
                                  <div class="time">{{ user.time }}</div>
                                  <div class="source">{{ user.from }}</div>
                                  <a-tag color="green">{{ user.zfmian }}</a-tag>
                              </a>
                          </div>
                      </a-tab-pane>
                      <a-tab-pane key="2" tab="正面" force-render>
                          <div class="list-item">
                              <a class="content-box" v-for="zhengmian in list2">
                                  <a class="content">{{ zhengmian.text }}</a>
                                  <div class="time">{{ zhengmian.time }}</div>
                                  <div class="source">{{ zhengmian.from }}</div>
                                  <a-tag color="green">{{ zhengmian.zmian }}</a-tag>
                              </a>
                          </div>
                      </a-tab-pane>
                      <a-tab-pane key="3" tab="负面">
                          <div class="list-item">
                              <a class="content-box" v-for="fumian in list3">
                                  <a class="content">{{ fumian.text }}</a>
                                  <div class="time">{{ fumian.time }}</div>
                                  <div class="source">{{ fumian.from }}</div>
                                  <a-tag color="red"> {{ fumian.fmian }} </a-tag>
                              </a>
                          </div>
                      </a-tab-pane>
                  </a-tabs>
                  <a-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
                      :current-page="currentPage" :page-sizes="6" :page-size="6"
                      layout="total, sizes, prev, pager, next, jumper" :total="98"
                      :show-total="total => `共 ${total} 页`" size="small"
                      style="display: flex;float: right;margin-top: 20px;">
                  </a-pagination>
              </a-tabel>
          </div>

          <!-- 表格2 -->
          <div class="pie margin">
              <a-table :columns="columns" :data-source="datatext2" :customHeaderRow="customHeaderRow"
                      :pagination="false" size="small" style="border: none;">
                  <template #title><b style="font-size: 16px;font-weight: 600;color: #000;">负面关键词top10</b></template>
              </a-table>
          </div>
      </div>
  </div>
</template>
<script>


//表格1和2 title
const columns = [
  {
      title: '排名',
      dataIndex: 'number',
      backgroundColor: '#e4e4e4',
      customRender: (value, row, index) => {
          const obj = {
              children: value,
              attrs: {},
          };
          obj.attrs.align = 'center';
          return obj;
      }
  },
  {
      title: '关键词',
      dataIndex: 'address',
      backgroundColor: '#e4e4e4',
      customRender: (value, row, index) => {
          const obj = {
              children: value,
              attrs: {},
          };
          obj.attrs.align = 'center';
          return obj;
      }
  },
  {
      title: '次数',
      dataIndex: 'much',
      backgroundColor: '#e4e4e4',
      //a-tabel 表格内容居中
      customRender: (value, row, index) => {
          const obj = {
              children: value,
              attrs: {},
          };
          obj.attrs.align = 'center';
          return obj;
      }
  },
];
//表格内容1
const datatext = [
  {
      number: '1',
      address: '高铁',
      much: '35',
  },
  {
      number: '2',
      address: '科技',
      much: '1',
  },
  {
      number: '3',
      address: '交通',
      much: '1',
  },
  {
      number: '4',
      address: '城市',
      much: '1',
  },
]
//表格内容2
const datatext2 = [
  {
      number: '1',
      address: '事故',
      much: '10',
  },
  {
      number: '2',
      address: '安检',
      much: '4',
  },
  {
      number: '3',
      address: '故障',
      much: '1',
  },
  {
      number: '4',
      address: '收费',
      much: '1',
  },
]
import * as echarts from 'echarts';
import "echarts-wordcloud";
export default {
  data() {
      return {
          //引入a-tabel数据
          datatext,
          columns,
          datatext2,
          //词云图
          chart: null,
          //引入圆环图数据
          pie_data: [
              { name: '正面', value: 38 },
              { name: '中性', value: 44 },
              { name: '负面', value: 16 }
          ],
          eltq: [
              { name: '恶劣天气', value: 326 },
              { name: '重大活动', value: 182 },
              { name: '运营安全', value: 110 }
          ],
          //全部新闻
          list: [
              { text: '长沙高铁站游客躲在柱子后面避雨', time: '2024-04-29 19:00:43', from: '今日头条', zfmian: '正面' },
              { text: '高铁地铁为什么不在夜间行车', time: '2024-04-29 18:00:33', from: '微博', zfmian: '正面' },
              { text: '马上评 | 高铁WIFI弹出骂人脏话，“无法处理”说不过去', time: '2024-04-29 16:00:43', from: '澎湃', zfmian: '正面' },
              { text: '高铁WIFI弹出骂人脏话国铁吉讯致歉', time: '2024-04-29 12:00:43', from: '今日头条', zfmian: '正面' },
              { text: '国铁吉讯就链接高铁WIFI弹出脏话致歉', time: '2024-04-29 5:00:43', from: '今日头条', zfmian: '正面' },
              { text: '高铁WIFI弹出骂人脏话国铁吉讯致歉', time: '2024-04-29 7:00:43', from: '今日头条', zfmian: '正面' },
              { text: '高铁WIFI弹出骂人脏话国铁吉讯致歉', time: '2024-04-29 3:00:43', from: '今日头条', zfmian: '正面' },
              { text: '高铁WIFI弹出骂人脏话，“无法处理”说不过去', time: '2024-04-29 11:00:43', from: '今日头条', zfmian: '正面' },
              { text: '高铁WIFI弹出骂人脏话，“无法处理”说不过去', time: '2024-04-29 11:00:43', from: '今日头条', zfmian: '正面' },
              { text: '上海地铁限时免收起步价', time: '2024-04-29 11:00:43', from: '微博', zfmian: '中性' },
              { text: '小男孩地铁上举了一路荣誉证书', time: '2024-04-29 11:00:43', from: '微博', zfmian: '中性' },
              { text: '小孩哥坐地铁手拿奖状展示一路', time: '2024-04-29 11:00:43', from: '百度', zfmian: '中性' },
          ],
          //正面新闻
          list2: [
              { text: '长沙高铁站游客躲在柱子后面避雨', time: '2024-04-29 19:00:43', from: '今日头条', zmian: '正面' },
              { text: '高铁地铁为什么不在夜间行车', time: '2024-04-29 19:00:43', from: '今日头条', zmian: '正面' },
              { text: '马上评丨高铁WiFi弹出骂人脏话，“无法处理”说不过去', time: '2024-04-29 19:00:43', from: '今日头条', zmian: '正面' },
              { text: '高铁WiFi弹出骂人脏话国铁吉讯致歉', time: '2024-04-29 19:00:43', from: '今日头条', zmian: '正面' },
              { text: '连高铁WIFI弹出脏话 12306称无法处理', time: '2024-04-29 19:00:43', from: 'QQ', zmian: '正面' },
              { text: '12306积分怎么买高铁外卖', time: '2024-04-29 19:00:43', from: '视频号', zmian: '正面' },
              { text: '12306积分可以买高铁外卖了', time: '2024-04-29 19:00:43', from: '微博', zmian: '正面' },
              { text: '为什么高铁霸座现象屡次发生？', time: '2024-04-29 19:00:43', from: '知乎', zmian: '正面' },
              { text: '中国城市高铁第一隧道开始盾构掘进', time: '2024-04-29 19:00:43', from: '微博', zmian: '正面' },
          ],
          //负面新闻
          list3: [
              { text: '地铁同站进出要收“起步费”？上海地铁新规：限时免收费', time: "2024-04-28 03:00:30", from: '澎湃', fmian: "负面" },
              { text: '马上评｜下飞机后乘地铁，“二次安检”太折腾', time: "2024-04-23 02:01:03", from: '澎湃', fmian: "负面" },
              { text: '部分飞机高铁换乘地铁需二次安检，为何仍无法实现安检互认？', time: "2024-04-28 04:00:30", from: '澎湃', fmian: "负面" },
              { text: '西安地铁10号线试车发生事故？官方通报：处置不当导致追尾', time: "2024-04-28 05:00:30", from: '澎湃', fmian: "负面" },
              { text: '西安地铁10号线试车追尾事故致1死2伤', time: "2024-04-28 06:00:30", from: '澎湃', fmian: "负面" },
              { text: '西安地铁试车时发生事故 ', time: "2024-04-28 07:00:30", from: '澎湃', fmian: "负面" },
          ],
      };
  },

  //词云图
  mounted() {
      this.initChart();
      this.getData();
  },

  //圆环图
  created() {
      this.initPie();
      this.initPie2();
  },

  methods: {

      //分页
      handleCurrentchange(currentPage) {
          this.currentPage = currentPage;
      },

      //页标签
      callback(key) {
          console.log(key);
      },

      //第一个圆环图
      initPie() {
          this.$nextTick(() => {
              let myChart = echarts.init(document.getElementById('main_pie'));
              let option = {
                  title: {
                      text: '热点正负面占比',
                      textStyle: {
                          fontSize: 16,
                          color: '#000',
                          fontWeight: 600,
                      }
                  },
                  tooltip: {},
                  color: ['#5cc93f', '#47a3f7', '#fb3c36'],
                  legend: {
                      //头部导航图形
                      icon: 'circle',
                      //设置图形的大小显示
                      itemWidth: 8,
                      itemHeight: 8,
                      top: '9%',
                      data: this.pie_data.map(item => {
                          return {
                              name: item.name,
                          };
                      })
                  },
                  series: [
                      {
                          name: '',
                          value: '',
                          legendHoverLink: true,
                          //外围内环圆角化
                          radius: ['40%', '55%'],
                          label: {
                              show: true,
                              formatter: '{d}%',
                              color: '#000'
                          },
                          itemStyle: {
                              borderWidth: 10,
                              borderColor: '#fff',
                          },
                          type: 'pie',
                          data: this.pie_data.map(item => {
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
      //第二个圆环图
      initPie2() {
          this.$nextTick(() => {
              let myMain = echarts.init(document.getElementById('main2'));
              let option2 = {
                  title: {
                      text: '热点主题统计',
                      textStyle: {
                          fontSize: 16,
                          color: '#000',
                          fontWeight: 600,
                      }
                  },
                  tooltip: {},
                  color: ['#75849e', '#f5bd41', '#5e8ded'],

                  legend: {
                      //头部导航图形
                      icon: 'circle',
                      //设置图形的大小显示
                      itemWidth: 8,
                      itemHeight: 8,
                      top: '9%',
                      data: this.eltq.map(item => {
                          return {
                              name: item.name,
                          };
                      })
                  },
                  series: [
                      {
                          legendHoverLink: true,
                          //外围内环圆角化
                          radius: ['40%', '55%'],
                          label: {
                              show: true,
                              formatter: '{d}%',
                              color: '#000'
                          },
                          itemStyle: {
                              borderWidth: 10,
                              borderColor: '#fff',
                          },
                          type: 'pie',
                          data: this.eltq.map(item => {
                              return {
                                  name: item.name,
                                  value: item.value
                              };
                          })
                      }
                  ],
              };
              myMain.setOption(option2);
          })
      },

      //a-tabel 表头的属性设置
      customHeaderRow() {
          return {
              style: {
                  fontSize: '15px',
                  fontWeight: '500',
                  backgroundColor: '#f0f2f5',
                  width: '100%'
              }
          }
      },

      //词云图
      initChart() {
          this.chart = echarts.init(document.getElementById("container"));
          var keywords = [
              {
                  "name": '高铁',
                  "value": 35
              },
              {
                  "name": '城市',
                  "value": 1
              }, {
                  "name": '收费',
                  "value": 1
              }, {
                  "name": '事故',
                  "value": 10
              }, {
                  "name": '暴雨',
                  "value": 3
              }, {
                  "name": '故障',
                  "value": 1
              }, {
                  "name": '科技',
                  "value": 1
              }, {
                  "name": '安检',
                  "value": 1
              }, {
                  "name": '交通',
                  "value": 1
              },
          ]

          var option3 = {
              tooltip: {
                  name: '',
                  value: '',
                  show: true,
                  matter: '{b} : {c}',
                  color: '#000',
                  borderWidth: 1,
                  padding: [10, 15, 10, 15],
                  confine: true,
              },
              series: [
                  {
                      type: 'wordCloud',
                      sizeRange: [25, 40],
                      rotationRange: [10, 0],
                      rotationStep: 20,
                      gridSize: 15,
                      width: '600px',
                      height: '400px',
                      left: 'center',
                      top: 'center',
                      shape: "pentagon",
                      drawOutOfBound: true,
                      textStyle: {
                          color: function () {
                              return (
                                  'rgb(' +
                                  [
                                      Math.round(Math.random() * 160),
                                      Math.round(Math.random() * 160),
                                      Math.round(Math.random() * 160)
                                  ].join(',') +
                                  ')'
                              )
                          }
                      },
                      emphasis: {
                          trigger: 'axis',
                          axisPointer: {
                              type: 'shadow'
                          },
                      },
                      itemStyle: {
                          borderWidth: 10,
                          borderColor: '#fff',
                      },
                      data: keywords,
                  }
              ]
          };
          this.chart.setOption(option3)
      },
  },
};
</script>

<style>
/* 总页面 */
.test2 {
  width: 100%;
  height: 100%;
  margin: 5px;
}

/* 头部导航栏 */
.header {
  background-color: white;
  height: 60px;
  line-height: 60px;
  font-size: 20px;
  color: #000000;
  padding-left: 10px;
  margin-right: 10px;
  font-weight: 600;
}

/* 头部导航栏的选择框 */
.select {
  float: right;
  width: 100px;
  color: #000000;
  margin-top: 15px;
  margin-right: 30px;
  font-weight: normal;
}

/* 第一个盒子 */
.pie {
  width: 400px;
  height: 400px;
  margin-top: 10px;
  background-color: #fff;
  margin-right: 10px;
  padding-left: 5px;
  padding-top: 5px;
}

/* 第二个盒子 */
.num {
  width: 690px;
  height: 400px;
  margin-top: 5px;
  background-color: #fff;
}


/* 第二个盒子导航栏
  图片+文字 */
.sum-item {
  display: flex;
  flex-flow: row nowrap;
  margin-left: 35px;
  gap: 12px;
}

/* 热点 */
.alt {
  margin-top: 20px;
  margin-left: 50px;
}

/* 热点数量 */
.riaghttop {
  font-weight: 400;
  font-size: 22px;
  margin-top: 20px;
  color: #3d3d3d;
  line-height: 22px;
}

/* 热点事件 */
.content-box {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  font-weight: 400;
  font-size: 12px;
  color: #666;
  align-items: center;
  padding: 8px;
}


/* 表格数据 */
.list-item {
  display: flex;
  flex-flow: column nowrap;
  overflow-y: auto;
  height: 240px;
  gap: 10px;
}

/* 新闻文本内容 */
.content {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  width: 63%;
  font-size: 14px;
  width: 350px;
  color: #3d3d3d;
  line-height: 14px;
}

/* 新闻文本设置链接，鼠标悬停颜色变化 */
a:hover {
  color: #1ea7ff;
}

/* 新闻出版时间 */
.time {
  margin-left: 40px;
  color: rgba(0, 0, 0, 0.4);
}

/* 新闻出版社 */
.source {
  color: red;
}

.margin{
  margin-left: 5px;
}
</style>