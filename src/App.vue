<template>
  <div class="container">
    <h1>Soccer Timer</h1>
    <p v-if="(secondTimer > mainTimer) &&(mainTimer > 0) " class="main-timer">{{showTime(mainTimer)}}</p>
    <p v-if="mainTimer === 0" class="error">Time out</p>
    <p  v-if="secondTimer > 0 " class="secondary-timer">{{showTime(secondTimer)}}</p>
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
  setup(){
    const mainTimer = ref(480)
    const secondTimer = ref(720)
    const running= ref(false)
    const mainTimerEnd = ref(0)
    const secondTimerEnd =ref(0)
    
    let interval
    const timerRunning = () => {
      
      mainTimerEnd.value = Date.now() + mainTimer.value * 1000
      secondTimerEnd.value = Date.now() + secondTimer.value * 1000

      running.value = true
      interval =  setInterval(()=>{
        if (mainTimer.value>0) {
          mainTimer.value = Math.ceil((mainTimerEnd.value - Date.now()) / 1000)
        }
         if (secondTimer.value>0) {
            secondTimer.value = Math.ceil((secondTimerEnd.value - Date.now()) / 1000)
        }
      },1000)
    }
    const addMinute = () => {
      mainTimerEnd.value = mainTimerEnd.value + 60000
      secondTimerEnd.value = secondTimerEnd.value + 60000
    }
    const stopTimer = () => {
      running.value = false
      clearInterval(interval)
    }
    const resetTimer = () => {
      running.value = false
      mainTimer.value = 480
      secondTimer.value = 720
      mainTimerEnd.value= 0
      secondTimerEnd.value = 0
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
      mainTimerEnd.value = Date.now() + 120000
      mainTimer.value = Math.ceil((mainTimerEnd.value - Date.now()) / 1000)    
    }
    return{ mainTimer, secondTimer, timerRunning, stopTimer, addMinute, resetTimer, showTime, running, addTwoMinutes}
  }
}
</script>

<style>

</style>
