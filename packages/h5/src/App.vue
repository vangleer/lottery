<template>
  <div class="app-container">
    <div class="l-lottery">
      <div class="l-box">
        <div class="l-circle-pan-box" >
          <div
            class="l-circle-pan-box-inner"
            :style="{ transform: `rotate(${state.degNum}deg)` }"
          >
            <div
              class="num-item"
              v-for="(item, index) in lotteryList"
              :key="index"
              :class="['num-item' + index]"
              :style="item"
            >
              <div class="num-item-name">{{ item.name }}</div>
              <span class="num-item-text">{{ item.desc }}</span>
            </div>
          </div>
          <img class="l-circle-btn" src="./assets/images/circle_btn.png" @click="handlePreStartClick" />
          <img class="l-head-icon" src="./assets/images/sanjiao.png" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'

  const lotteryList = reactive([
    { name: 'IPhone14', desc: '手机' },
    { name: '100元', desc: '话费' },
    { name: '再来一次', desc: '鼓励奖' },
    { name: '10元', desc: '话费' },
    { name: '5元', desc: '话费' },
    { name: '20元', desc: '话费' },
    { name: '笑脸', desc: '谢谢参与' },
    { name: 'IPad Air', desc: '平板' }
  ])

  const state = reactive({
    isStart: false,
    circleNum: 0,
    currentLottery: {},
    degNum: 180
  })
  function getStyle() {
    const w = 236 // 原型的宽度
    const len = lotteryList.length
    //中心点横坐标
    var dotLeft = (w - 95) / 2;
    //中心点纵坐标
    var dotTop = (w - 100) / 2;
    var radius = w / 2
    var avd = 360 / len
    var ahd = avd * Math.PI / 180;
    lotteryList.forEach((item: any, index) => {
      item.left = Math.sin(ahd * index) * radius + dotLeft + 'px'
      item.top = Math.cos(ahd * index) * radius + dotTop + 'px'
      let deg = 180 - (index * 45) + index + 'deg'
      
      item.transform = `rotate(${deg})`
    })
  }
  function startClick() {
			if(!state.isStart) {
        state.isStart = true

				state.circleNum += 7
				state.isStart = true

				const index = Math.floor(Math.random() * lotteryList.length)
				console.log(index)
				state.degNum = state.circleNum * 360 + (index * 45) + 180

				const current = lotteryList[index]

        setTimeout(() => {
          alert(`恭喜您，获得了${current.name}${current.desc}`)
        }, 4500)
        state.isStart = false
			}
		}

  getStyle()

  function handlePreStartClick() {
    startClick()
  }
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body, html, #app, .app-container {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.l-lottery {
  position: relative;
  width: 375px;
  height: 100%;
  background: url('./assets/images/background_cj.png') no-repeat;
  background-size: 100% auto;
  .l-box {
    width: 100%;
    flex-direction: row;
    justify-content: center;
    height: 350px;
  }
  .l-circle-pan-box {
    position: absolute;
    left: 50%;
    top: 223px;
    width: 236px;
    height: 236px;
    border-radius: 50%;
    transform: translateX(-50%);
  }
  .l-circle-pan-box-inner {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: url('./assets/images/circle_pan_blank.png') no-repeat;
    background-size: 100% 100%;
    transition: all 4s;
  }
  .l-circle-btn {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -55%);
    width: 95px;
    height: 100px;
  }
  .l-head-icon {
    position: absolute;
    left: 50%;
    top: 0px;
    transform: translate(-53%, -60%);
    width: 100px;
    height: 80px;
    width: 86px;
	  height: 77px;
  }
  .num-item {
    position: absolute;
    width: 100px;
    height: 100px;
    // background-color: red;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding-top: 80px;
  }
  .num-item-name {
    color: #e73454;
    font-size: 12px;
    font-weight: 700;
  }
  .num-item-text {
    color: #b55e28;
    font-size: 13px;
    margin-top: 6px;
    font-weight: 700;
  }
}
</style>
