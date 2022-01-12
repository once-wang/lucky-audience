<template>
  <div class="home">
    <el-container class="inner">
      <el-header>
        <div class="page-header">
          <span class="item" @click="goWork">生</span>
          <span class="item" @click="goEat">活</span>
        </div>
      </el-header>
      <el-main>
        <eat v-if="pagechange === 'eat'"></eat>
        <work v-if="pagechange === 'work'"></work>
      </el-main>
    </el-container>
  </div>
</template>

<script>
// @ is an alias to /src
import Eat from "@/views/Eat";
import Work from "@/views/Work";
export default {
  name: 'Home',
  components: {
    Eat,
    Work
  },
  data() {
    return {
      pagechange: ''
    }
  },
  methods: {
    goWork() {
      this.pagechange = 'work'
    },
    goEat() {
      this.pagechange = 'eat'
    }
  }
}
</script>

<style scoped>
.home {
  width: 100%;
  height: 100%;
  padding: 0;
  position: fixed;
  background-image: url('../assets/background.jpg');
  background-size: 100% 100%;
  z-index: 1;
}

.home:after {
  content: '';
  height: 100%;
  width: 100%;
  position: absolute;
  left: 0;
  top: 0;
  background: inherit;
  filter: blur(5px);
  z-index: 2;
}

.inner {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 10;
}

.page-header {
  position: absolute;
  top: 0;
  left: 0;
  padding-left: 0.52083rem;
  width: 100%;
  color: #fff;
  z-index: 100;
}

.item {
  margin-left: 40px;
  font-size: 30px;
  cursor: pointer;
  width: 5rem;
  height: 5rem;
  line-height: 5rem;
  color: #fff;
}

.item:hover {
  background-color:wheat
}

</style>