<template>
  <div>
    <!-- <el-progress :percentage="percentage" :color="customColor"></el-progress>

    <el-progress
      :percentage="percentage"
      :color="customColorMethod"
    ></el-progress>

    <el-progress :percentage="percentage" :color="customColors"></el-progress>
    <div>
      <el-button-group>
        <el-button icon="el-icon-minus" @click="decrease"></el-button>
        <el-button icon="el-icon-plus" @click="increase"></el-button>
      </el-button-group>
    </div> -->
    <el-progress
      ref="message"
      type="circle"
      :percentage="data1"
      :status="ddd"
    ></el-progress>
    <!-- <el-progress type="circle" :percentage="25"></el-progress>
    <el-progress type="circle" :percentage="100" status="success"></el-progress>
    <el-progress type="circle" :percentage="70" status="warning"></el-progress>
    <el-progress
      type="circle"
      :percentage="50"
      status="exception"
    ></el-progress> -->
  </div>
</template>

<script>
import "element-ui/lib/theme-chalk/index.css";

export default {
  components: {},
  data() {
    return {
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
    };
  },
  mounted() {
    this.dd();
  },
  //   watch: {
  //     dd() {
  //       for (let i = 0; i <= 100; i++) {
  //         this.data1 = i;
  //       }
  //       this.ddd = "success";
  //     },
  //   },
  methods: {
    dd() {
      for (let i = 0; i <= 100; i++) {
        setTimeout(() => {
          this.data1 = i;
          console.log(this.data1);
        }, (this.dddd += 1000));
      }

      //   setTimeout(() => {
      //     this.ddd = "success";
      //   }, this.data1 == 100);
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
  },
};
</script>

<style>
</style>