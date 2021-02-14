<template>
  <body>
    
    <div style="float:left; margin-left:42%;">
      <h1>노예 시급 계산기</h1>
      <div class="something-semantic">
        <div class="something-else-semantic">
          {{timestamp}}
        </div>
      </div>
      <br>
      <div>
        <label for>연봉(단위:만원):</label>
        <input type="number" v-model="value" @change="onChange">
      </div>
    </div>

    
    <div style="margin-right:20%;">
      <img src="../assets/곡괭이질.gif" style="width:200px; height:150px;">
    </div>

    <div class="wrapper">
      <div class="counter col_fourth">
        <i class="fa fa-code fa-2x"></i>
        <!-- <h2 class="timer count-title count-number" data-to="300" data-speed="1500"></h2> -->
        <animated-number :value="value*10000/231/32400*daysecond" :formatValue="formatToPrice" :duration="duration"/>
        <p class="count-text ">daily</p>
      </div>

      <div class="counter col_fourth">
        <i class="fa fa-coffee fa-2x"></i>
        <h2 class="timer count-title count-number" data-to="1700" data-speed="1500"></h2>
        <animated-number :value="(coffee*10000/231)/4500" :formatValue="formatToCoffe" :duration="duration"/>
        <p class="count-text ">coffee</p>
      </div>

      <div class="counter col_fourth">
        <i class="fa fa-lightbulb-o fa-2x"></i>
        <h2 class="timer count-title count-number" data-to="11900" data-speed="1500"></h2>
        <animated-number :value="value*10000/12" :formatValue="format" :duration="duration"/>
        <p class="count-text ">month</p>
      </div>

      <div class="counter col_fourth end">
        <i class="fa fa-bug fa-2x"></i>
        <h2 class="timer count-title count-number" data-to="157" data-speed="1500"></h2>
        <animated-number :value="value*10000" :formatValue="format" :duration="duration"/>
        <p class="count-text ">total</p>
      </div>
      <!-- <el-progress
        ref="message"
        type="circle"
        :percentage="data1"
      ></el-progress> -->

      <!-- <canvas id="myChart" width="400" height="400"></canvas> -->
      <br>
      <div class="something-semantic">
        <div class="something-else-semantic">
        </div>
      </div>

      <br>
      <div style="float:left;">
        <GChart
          type="ColumnChart"
          :data="dayChartData"
          :options="chartOptions"
        />
      </div>
      <div style="float: right;">
        <GChart
          type="ColumnChart"
          :data="chartData"
          :options="chartOptions"
        />
      </div>

    </div>
    
  </body>
</template>


<script>
import AnimatedNumber from "animated-number-vue";
import moment from "moment";
import { GChart } from 'vue-google-charts';
import "element-ui/lib/theme-chalk/index.css";

export default {
  name: "App",  
  components: {
    AnimatedNumber,
    GChart,
  },
  name: 'HelloWorld',
  data () {
    return {
      value: 5000,
      coffee: 5000,
      duration: 3000,
      time: "",
      date: "",
      daysecond: 0,
      timestamp: "",
      chartData: [],
      dayChartData: [],
      chartOptions: {
          chart: {
          title: 'Company Performance',
          subtitle: 'Sales, Expenses, and Profit: 2014-2017',
        }
      },
      data1: 0,
      ddd: "",
      dddd: 1000,
      percentage: 20,
      customColor: "#409eff",
      customColors: [
        { color: "#f56c6c", percentage: 20 },
        { color: "#e6a23c", percentage: 40 },
        { color: "#5cb87a", percentage: 60 },
        { color: "#1989fa", percentage: 80 },
        { color: "#6f7ad3", percentage: 100 },
      ],
    }
  },
  mounted() {
    this.dd();
  },
  computed: {
  },
  created () {
    this.interval();
    this.watch();
  },
  methods: {

    dd() {
      // for (let i = 0; i <= 100; i++) {
      //   setTimeout(() => {
      //     this.data1 = i;
      //     console.log(this.data1);
      //   }, (this.dddd += 100));
      // }
    },
    customColorMethod(percentage) {
      if (percentage < 30) {
        return "#909399";
      } else if (percentage < 70) {
        return "#e6a23c";
      } else {
        return "#67c23a";
      }
    },
    increase() {
      this.percentage += 10;
      if (this.percentage > 100) {
        this.percentage = 100;
      }
    },
    decrease() {
      this.percentage -= 10;
      if (this.percentage < 0) {
        this.percentage = 0;
      }
    },

    formatToPrice(value) {
      var regexp = /\B(?=(\d{3})+(?!\d))/g;
      var temp = Number(value).toFixed(2);
      return `<h6>￦${temp.toString().replace(regexp, ',')}</h6>`;
    },

    formatToCoffe(value) {
      var regexp = /\B(?=(\d{3})+(?!\d))/g;
      var temp = Number(value).toFixed(0);
      return `<h6>${temp.toString().replace(regexp, ',')}cup</h6>`;
    },

    format(value) {
      var regexp = /\B(?=(\d{3})+(?!\d))/g;
      var temp = Number(value).toFixed(0);
      return `<h6>${temp.toString().replace(regexp, ',')}</h6>`;
    },

    interval(){
      
      const today = new Date();
      var date = moment().format("YYYY-MM-DD");
      var start = moment(date+" 09:00:00", "YYYY-MM-DD HH:mm:ss");
      var current = moment();
      var second = moment.duration(current.diff(start)).asSeconds();
      this.daysecond = second;

      if(9 <= today.getHours() && today.getHours() <= 18 && today.getDay()!=6 && today.getDay()!=7){
        
        setInterval(() => {          
          this.dayChartData = new Array();
          this.chartData = new Array();
          current = moment();

          console.log(moment.duration(current.diff(date)).asDays())

          if(moment.duration(current.diff(date)).asDays() >= 1){
            this.daysecond = 0;
            this.coffee = 0;
          }else{
            second = moment.duration(current.diff(start)).asSeconds();
            this.daysecond = second;
            this.coffee=this.value;
          }
          
          
          var chart = new Array();
          chart.push("NAME");
          chart.push("current");
          chart.push("MAX");

          var section = new Array();
          section.push("daily");
          section.push(this.value*10000/231/32400*this.daysecond);
          section.push(this.value*10000/231/32400);
          
          var section2 = new Array();
          section2.push("coffee");
          section2.push((this.coffee*10000/231)/4500);
          section2.push((this.coffee*10000/231)/4500);

          var section3 = new Array();
          section3.push("month");
          section3.push((this.coffee*10000/231)/4500);
          section3.push(this.value*10000/12);

          var section4 = new Array();
          section4.push("total");
          section4.push((this.coffee*10000/231)/4500);
          section4.push(this.value*10000);

          this.dayChartData.push(chart);
          this.chartData.push(chart);
          this.dayChartData.push(section);
          this.dayChartData.push(section2);
          this.chartData.push(section3);
          this.chartData.push(section4);
        }, 5000)
      }else{
        this.daysecond = 0;
        this.coffee = 0;

        var chart = new Array();
        chart.push("NAME");
        chart.push("current");
        chart.push("MAX");

        var section = new Array();
        section.push("daily");
        section.push(this.value*10000/231/32400*this.daysecond);
        section.push(this.value*10000/231);
        

        var section2 = new Array();
        section2.push("coffee");
        section2.push((this.coffee*10000/231)/4500);
        section2.push((this.coffee*10000/231)/4500);

        var section3 = new Array();
        section3.push("month");
        section3.push((this.coffee*10000/231)/4500);
        section3.push(this.value*10000/12);

        var section4 = new Array();
        section4.push("total");
        section4.push((this.coffee*10000/231)/4500);
        section4.push(this.value*10000);

        this.dayChartData.push(chart);
        this.chartData.push(chart);
        this.dayChartData.push(section);
        this.dayChartData.push(section2);
        this.chartData.push(section3);
        this.chartData.push(section4);
      }
    },

    watch(){
      setInterval(() => {
        const today = new Date();
        const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
        const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
        const dateTime = date +' '+ time;
        this.timestamp = dateTime;
      }, 1000)
    },
    onChange(){
      this.interval();
    }
  },
}

</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
  font-family: Arial;
  padding: 25px;
  background-color: #f5f5f5;
  color: #808080;
  text-align: center;	
}

/*-=-=-=-=-=-=-=-=-=-=-=- */
/* Column Grids */
/*-=-=-=-=-=-=-=-=-=-=-=- */

.col_half { width: 49%; }
.col_third { width: 32%; }
.col_fourth { width: 23.5%; }
.col_fifth { width: 18.4%; }
.col_sixth { width: 15%; }
.col_three_fourth { width: 74.5%;}
.col_twothird{ width: 66%;}
.col_half,
.col_third,
.col_twothird,
.col_fourth,
.col_three_fourth,
.col_fifth{
	position: relative;
	display:inline;
	display: inline-block;
	float: left;
	margin-right: 2%;
	margin-bottom: 20px;
}
.end { margin-right: 0 !important; }
/* Column Grids End */

.wrapper { width: 980px; margin: 30px auto; position: relative;}
.counter { background-color: #ffffff; padding: 20px 0; border-radius: 5px;}
.count-title { font-size: 40px; font-weight: normal;  margin-top: 10px; margin-bottom: 0; text-align: center; }
.count-text { font-size: 18px; font-weight: bold;  margin-top: 10px; margin-bottom: 0; text-align: center; }
.fa-2x { margin: 0 auto; float: none; display: table; color: #4ad1e5; }


* {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
}

span {
  font-size: 50px;
}

button {
  border: none;
  margin-left: 20px;
  padding: 10px;
  border-radius: 100px;
}
button:focus {
  outline: none;
}

.something-semantic {
    display: table;
    width: 100%;
}

.something-else-semantic {
    display: table-cell;
    text-align: center;
    vertical-align: middle;
}

</style>
