<template>
  <div style="width: 100%; height: 100%;" align="center">
    <div v-for="(item, index) in pieces" :key="index">
      <el-row style="margin-top: 10px">
        <el-col :span="1"><span>选项名</span></el-col>
        <el-col :span="3" style="margin-left: 20px"><el-input v-model="item.name"></el-input></el-col>
        <el-col :span="1" style="margin-left: 20px"><span>选项值</span></el-col>
        <el-col :span="3" style="margin-left: 20px"><el-input v-model="item.value"></el-input></el-col>
        <el-col :span="3" style="margin-left: 20px"><el-button type="danger" @click="deleteItem(index)">删除选项</el-button></el-col>
      </el-row>
    </div>
    <div style="margin-top: 10px; text-align: left; padding-left: 200px">
      <el-button @click="addItem">新增选项</el-button>
      <el-button type="primary" @click="myEcharts" style="margin-left: 20px">生成转盘</el-button>
    </div>
    <div class="rotate" style="width: 500px; height: 500px; margin-top: 50px; magin-left: 50px">
      <div class="arrow" style="width: 100%; height: 100%">
        <div class="wheel" id="wheel" :style="{transform: `rotate(${currentAngle}deg)`}"></div>
        <div class="pointer" @click="startArrow"></div>
      </div>
    </div>
    <div class="text" v-if="currentAward">{{ currentAward }}</div>
  </div>
</template>

<script>
export default {
  name: "Eat",
  props: {
    msg: String
  },
  data() {
    return {
      currentAngle: 0,
      times: 0,
      currentAward: '等待抽奖',
      pieces: [
        // { value: 1, name: '沙县' },
        // { value: 1, name: '麻辣烫' },
        // { value: 1, name: '山西面' },
        // { value: 1, name: '小碗菜' },
        // { value: 1, name: '华科士' },
        // { value: 1, name: '麻辣香锅' }
      ]
    }
  },
  // mounted() {
  //   this.myEcharts();
  // },
  methods: {
    myEcharts() {
      let myChart = this.$echarts.init(document.getElementById('wheel'));

      let option = {
        // title: {
        //   text: '干饭人干饭魂',
        //   subtext: '午饭',
        //   left: 'center'
        // },
        // tooltip: {
        //   trigger: 'item'
        // },
        // legend: {
        //   orient: 'vertical',
        //   left: 'left'
        // },
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: '100%',
            label: {
              position: 'inside'
            },
            data: this.pieces,
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      };

      myChart.setOption(option);
    },
    addItem() {
      this.pieces.push({
        value: '',
        name: ''
      })
    },
    deleteItem(index) {
      if(index !== -1)
        this.pieces.splice(index, 1)
    },
    startArrow() {
      if (this.currentAward === '正在抽奖') return
      let award = this.getAward()
      this.getAngle(award)
      this.setCurrentAward(award)
    },
    getAward() {
      let randomNum = Math.random() * this.pieces.length
      console.log(randomNum)
      return parseInt(randomNum % this.pieces.length, 10)
    },
    getAngle(award) {
      this.times++
      let single = 360 / this.pieces.length
      let min = award * single
      let angle = parseInt(Math.random() * single + min, 10)
      console.log(angle)
      this.currentAngle = 360 * (this.times * 7) - angle
    },
    setCurrentAward(award) {
      this.currentAward = '正在抽奖'
      setTimeout(() => {
        console.log(award)
        console.log(this.pieces)
        this.currentAward = this.pieces[award].name
      }, 5000)
    }
  }
}
</script>

<style scoped>
.arrow {
  position: relative;
}
.wheel {
  transition: transform 5s;
  position: relative;
  width: 100%;
  height: 100%;
}
.pointer {
  background-repeat: no-repeat;
  background-position: center;
  background-image: url('../assets/arrow.png');
  content: '';
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  position: absolute;
}
.text {
  margin-top: 20px;
  font-size: 26px;
  font-weight: bold;
}
</style>