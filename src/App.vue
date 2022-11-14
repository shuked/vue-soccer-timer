<template>
<div class="container">
  <h1>Soccer Timer</h1>
 <p v-if="mainTimer > 0" class="main-timer">{{showTime(mainTimer)}}</p>
 <p v-if="mainTimer === 0" class="error">Time out</p>
 <p  v-if="secondTimer > 0" class="secondary-timer">{{showTime(secondTimer)}}</p>
 <p v-if="secondTimer === 0" class="error">Stop the game</p>
 <button v-if="!running" @click="timerRunning">start</button>
 <button v-if="running" @click="stopTimer">stop</button>
 <button @click="addMinute">add Minute</button>
 <button @click="resetTimer">reset</button>
 <button v-if="mainTimer === 0" @click="addTwoMinutes">add 2 Minutes</button>

</div>
 
</template>

<script>
import { ref } from '@vue/reactivity'


export default {
  name: 'App',
  components: {
    
  },
  setup(){
    const mainTimer = ref(480)
    const secondTimer = ref(720)
    const running= ref(false)

    

    let interval
    const timerRunning = () => {
      
    
      running.value = true
    interval =  setInterval(()=>{
        if (mainTimer.value>0) {
          mainTimer.value = mainTimer.value -1
        }
        
         if (secondTimer.value>0) {
          secondTimer.value = secondTimer.value -1
        }
      },1000)
    }
    const addMinute = () => {
      mainTimer.value = mainTimer.value + 60
      secondTimer.value = secondTimer.value + 60
    }
    const stopTimer = () => {
      running.value = false
      clearInterval(interval)
    }

    const resetTimer = () => {
      running.value = false
      mainTimer.value = 480
      secondTimer.value = 720
      clearInterval(interval)

    }

    const showTime = (sec) => {
    const minutes = Math.floor(sec/60)
    const seconds = sec%60

    const showMinutes = minutes.toString().length === 1 ? `0${minutes}` : `${minutes}`
    const showSeconds = seconds.toString().length === 1 ? `0${seconds}` : `${seconds}`

    return `${showMinutes}:${showSeconds}`
}

  const addTwoMinutes = () => {
      mainTimer.value = mainTimer.value + 120
  }

    return{ mainTimer, secondTimer, timerRunning, stopTimer, addMinute, resetTimer, showTime, running, addTwoMinutes}
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
