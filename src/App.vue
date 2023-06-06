<script setup>
  import { ref, onMounted, onUpdated, watch } from 'vue'

  const count = ref(0)
  const toggle = ref(false)
  let ctx = ''
  let decayInterval = ''

  function decay() { 
    decayInterval = setInterval( () => {
    if (count.value > 0) {
      count.value--
    } else {
      count.value = 0
    }
    drawBar(count.value)
  }, 250)}
  

  function add() {
    count.value++
    drawBar(count.value)

    if (count.value >= 20) {
      toggle.value = true
      clearInterval(decayInterval)
      document.getElementById("hitButton").disabled = true
    }

  }

  function drawBar(num) {
    ctx.reset()
    ctx.fillStyle = "white"
    ctx.fillRect(0, 0, num*10, 50)
  }

  onMounted(() => {
    ctx = document.getElementById("c").getContext("2d")
    decay()
  })
</script>

<template>
  <div id="allContainer">
    <Transition name="fade" mode="out-in">
      <div id="start" v-if="!toggle">
        <div id='Bar'>
          <canvas id="c"></canvas>
        </div>

        <div id="button">
          <button id="hitButton" @click="add(10)">Hit me</button>
        </div>
      </div>

      <div id="end" v-else>
        <img src="./assets/shinji.jpg">
      </div>
    </Transition>
  </div>
</template>

<style>
body {
  background-color: darkslategrey;
}

button {
  position: relative;
  display: inline-block;
}

#allContainer {
  width: 100%;
  height: 100%;
  margin: auto;
}

#start {  display: grid;
  grid-template-columns: 0.8fr 1.5fr 0.7fr;
  grid-template-rows: 0.5fr 0.5fr;
  grid-auto-columns: 1fr;
  gap: 0px 0px;
  grid-auto-flow: row;
  grid-template-areas:
    "Bar Bar Bar"
    ". button .";
}

#Bar { grid-area: Bar; }

#button { grid-area: button; }


#start, #end {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.fade-leave-active {
  transition: opacity 5s ease-out;
}

.fade-enter-active {
  transition: opacity 10s ease-in;
}

.fade-enter-from ,
.fade-leave-to {
  opacity: 0;
}
</style>
