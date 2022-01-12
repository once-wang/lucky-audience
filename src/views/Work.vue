<template>
  <div style="width: 100%; height: 100%;" align="center">
    <el-row>
      <el-col :span="12">
        <div v-for="(person, pindex) in people" :key="pindex">
          <el-row style="margin-top: 10px">
            <el-col :span="1"><span>姓名</span></el-col>
            <el-col :span="5" style="margin-left: 20px"><el-input v-model="person.name"></el-input></el-col>
            <el-col :span="5" style="margin-left: 20px"><el-button type="danger" @click="deletePerson(pindex)">删除人员</el-button></el-col>
          </el-row>
        </div>
      </el-col>
      <el-col :span="12">
        <div v-for="(task, tindex) in tasks" :key="tindex">
          <el-row style="margin-top: 10px">
            <el-col :span="2"><span>任务名</span></el-col>
            <el-col :span="5" style="margin-left: 20px"><el-input v-model="task.name"></el-input></el-col>
            <el-col :span="5" style="margin-left: 20px"><el-button type="danger" @click="deleteTask(tindex)">删除任务</el-button></el-col>
          </el-row>
        </div>
      </el-col>
    </el-row>
    <div style="margin-top: 10px; text-align: left; padding-left: 200px">
      <el-button @click="addPerson">新增人员</el-button>
      <el-button @click="addTask">新增任务</el-button>
      <el-button type="primary" @click="showTurnTable" style="margin-left: 20px">生成转盘</el-button>
    </div>
    <div class="rotate" style="width: 600px; height: 600px; margin-top: 50px; magin-left: 50px">
      <div class="arrow" style="width: 100%; height: 100%">
        <div class="wheel" id="wheel" :style="{transform: `rotate(${currentAngle}deg)`}"></div>
        <div class="ring" id="ring" @click="startArrow"></div>
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
      turntableRead: false,
      currentAward: '等待抽奖',
      tasks: [
        // { value: 1, name: '任务1' }
      ],
      people: [
        // { value: 1, name: '张三' }
      ]
    }
  },
  // mounted() {
  //   this.myEcharts();
  // },
  methods: {
    myEcharts() {
      let myChart = this.$echarts.init(document.getElementById('wheel'));
      let myChart1 = this.$echarts.init(document.getElementById('ring'));

      let speople = Object.assign([], this.people);
      let stasks = Object.assign([], this.tasks);

      this.shuffle(speople);
      this.shuffle(stasks);

      let option = {
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: '70%',
            label: {
              fontSize: '15',
              position: 'inside',
              width: 80,
              overflow: 'break'
            },
            data: stasks
          }
        ]
      };

      let option1 = {
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: ['70%', '100%'],
            avoidLabelOverlap: false,
            label: {
              position: 'inside',
              width: 80,
              overflow: 'break'
            },
            itemStyle: {
              borderRadius: 15,
              borderColor: '#fff',
              borderWidth: 2
            },
            emphasis: {
              label: {
                show: true,
                fontSize: '30',
                fontWeight: 'bold'
              }
            },
            labelLine: {
              show: false
            },
            data: speople
          }
        ]
      };

      myChart.setOption(option);
      myChart1.setOption(option1);
    },
    addPerson() {
      this.people.push({
        value: 1,
        name: ''
      })
    },
    addTask() {
      this.tasks.push({
        value: 1,
        name: ''
      })
    },
    deletePerson(index) {
      if(index !== -1)
        this.people.splice(index, 1)
    },
    deleteTask(index) {
      if(index !== -1)
        this.tasks.splice(index, 1)
    },
    startArrow() {
      if(this.turntableRead == true) {
        if (this.currentAward === '正在抽奖') return
        let award = this.getAward()
        this.getAngle(award)
        this.setCurrentAward(award)
      }
    },
    getAward() {
      let randomNum = Math.random() * this.people.length
      console.log(randomNum)
      return parseInt(randomNum % this.people.length, 10)
    },
    getAngle(award) {
      this.times++
      let single = 360 / this.people.length
      let angle = award * single
      console.log(angle)
      this.currentAngle = 360 * (this.times * 7) - angle
    },
    setCurrentAward(award) {
      this.currentAward = '正在抽奖'
      setTimeout(() => {
        console.log(award)
        console.log(this.people)
        this.currentAward = '任务分配完成'
      }, 5000)
    },
    showTurnTable() {
      if(this.people.length == this.tasks.length) {
        this.myEcharts();
        this.turntableRead = true;
      }
      else
        this.$message.error('error: 人员数量和任务数量不相等')
    },
    shuffle(arr) {
      let l = arr.length
      let index, temp
      while(l>0){
          index = Math.floor(Math.random()*l)
          temp = arr[l-1]
          arr[l-1] = arr[index]
          arr[index] = temp
          l--
      }
      return arr
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
.ring {
  /* background-repeat: no-repeat;
  background-position: center;
  background-image: url('../assets/arrow.png'); */
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