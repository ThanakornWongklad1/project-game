
<script setup>
import { ref } from 'vue'

const colorArray = [
  {
    "wrongColor": "bg-red-700",
    "rightColor": "bg-red-800"
  },
  {
    "wrongColor": "bg-blue-600",
    "rightColor": "bg-blue-700"
  },
  {
    "wrongColor": "bg-green-400",
    "rightColor": "bg-green-500"
  },
  {
    "wrongColor": "bg-yellow-400",
    "rightColor": "bg-yellow-500"
  },
  {
    "wrongColor": "bg-purple-600",
    "rightColor": "bg-purple-500"
  },
  {
    "wrongColor": "bg-pink-900",
    "rightColor": "bg-rose-900"
  },
  {
    "wrongColor": "bg-indigo-800",
    "rightColor": "bg-indigo-700"
  },
  {
    "wrongColor": "bg-amber-400",
    "rightColor": "bg-yellow-400"
  },
  {
    "wrongColor": "bg-emerald-200",
    "rightColor": "bg-green-200"
  },
  {
    "wrongColor": "bg-indigo-700",
    "rightColor": "bg-blue-800"
  },
  {
    "wrongColor": "bg-teal-800",
    "rightColor": "bg-teal-900"
  },
  {
    "wrongColor": "bg-slate-600",
    "rightColor": "bg-gray-600"
  }
]


let score = ref(0)
let colorSelected = []
let columNo = 4

const randomColor = () => {
  if (score.value >= 5 && score.value < 15) {
    columNo = 9
  }
  if (score.value >= 15 && score.value < 25) {
    columNo = 16
  }
  if (score.value >= 25) {
    columNo = 25
  }
  let color = colorArray[Math.floor(Math.random() * colorArray.length)];
  for (let i = 0; i < columNo; i++) {
    if (colorSelected.length < columNo - 1) {
      colorSelected.push(color.rightColor)
    }
    else {
      colorSelected.push(color.wrongColor)
      break;
    }
  }
  return colorSelected
}

const shuffleColor = () => {
  colorSelected = []
  let colorArr = randomColor()
  colorArr.sort((a, b) => 0.5 - Math.random())
  return colorArr
}

colorSelected = shuffleColor()

const checkAnswer = (ans) => {
  for (let i = 0; i < colorArray.length; i++) {
    if (ans == colorArray[i].wrongColor) {
      score.value++
      colorSelected = shuffleColor()
      break
    }
  }
}

const nextLevel = () => {
  if (score.value == 0) {
    return 'grid-cols-2'
  }
  if (score.value >= 5 && score.value <= 14) {
    return 'grid-cols-3'
  }
  if (score.value >= 15 && score.value <= 24) {
    return 'grid-cols-4'
  }
  if (score.value >= 25) {
    return 'grid-cols-5'
  }
  return 'grid-cols-2'
}

const scaling = () => {
  if (columNo == 4) {
    return 'w-40 h-40'
  }
  if (columNo == 9) {
    return 'w-28 h-28'
  }
  if (columNo == 16) {
    return 'w-20 h-20'
  }
  if (columNo == 25) {
    return 'w-16 h-16'
  }
}

let start = ref(false);
let countdown = ref(60);
let gameEnd = ref(false);
let timer;

const startGame = () => {
  if (start.value) return;
  start.value = true;
  gameEnd.value = false;
  timer = setInterval(() => {
    if (countdown.value === 0) {
      gameEnd.value = true
      return;
    }
    countdown.value--
  }, 1000)
}

const resetGame = () => {
  start.value = false;
  countdown.value = 60;
  score.value = 0;
  columNo = 4
  colorSelected = shuffleColor()
  clearInterval(timer);
  startGame()
}

const input = ref('')

let mainMenu = ref(true)
let gamePage = ref(false)

const setMain = () => {
  mainMenu.value = !mainMenu.value
  gamePage.value = !gamePage.value
  gameEnd.value = false
  countdown.value = 60;
  score.value = 0;
  columNo = 4
  colorSelected = shuffleColor()
  startGame()
}


</script>
 
<template>

  <body class="bg-gray-800 w-full h-screen">
    <!-- main menu -->
    <div class="flex-col" v-show="mainMenu">
      <div>
        <img src="./Icon/Logo.png" alt="logo" class="w-2/5 mx-auto w-">
      </div>
      <div class="w-1/3 h-80 rounded-3xl bg-slate-700 mt-12 mx-auto">
        <div class="flex flex-col">
          <input type="text" placeholder="Type your name..." v-model="input" @keyup.enter="setMain()"
            class="mx-auto mt-10 px-5 rounded-lg w-1/2 h-12" />
          <button class="mx-auto mt-7 w-1/2 h-12 bg-slate-900 text-white rounded-lg hover:bg-slate-800"
            @click="setMain()">Let's
            Start!</button>
          <hr class="mt-7 border-slate-500 w-4/5 mx-auto">
          <label for="my-modal"
            class="mx-auto mt-7 w-1/2 h-12 bg-slate-900 text-white rounded-lg hover:bg-slate-800 text-center pt-3">How
            to play</label>
        </div>
      </div>
    </div>

    <!-- how to -->
    <input type="checkbox" id="my-modal" class="modal-toggle" />
    <div class="modal">
      <div class="modal-box bg-sky-900 text-white">
        <h3 class="font-bold text-2xl text-center">How to play</h3>
        <p class="py-4 text-center">Click on the different shade of color in a short period of time. <br>
          Once you find the different shade, <br>
          you click it and score one point if it is the different one.</p>
        <div class="modal-action">
          <label for="my-modal" class="btn bg-cyan-700">I got it !!</label>
        </div>
      </div>
    </div>

    <!-- game page -->
    <div class="flex flex-col">
      <div v-show="gamePage" :hidden="gameEnd">
        <img src="./Icon/Logo.png" alt="logo" width="200" class="m-4">
        <div class="flex flex-row justify-center">
          <!-- interface score and user -->
          <div class="basis-1/2 flex justify-end">
            <span class=" text-2xl text-white mt-20">Player : {{ input== '' ? 'Guest' : input }} <br>Score : {{
              score
            }}</span>
          </div>
          <!-- interface game -->
          <div class="flex flex-col  basis-1/2">
            <div style="width:400px;height: 500px;" class="bg-slate-500 rounded-3xl mt-5 mx-24 flex flex-col">
              <div class="text-center text-4xl mt-5 text-white">{{ countdown }}</div>
              <div class="grid gap-2 mx-auto mt-10" :class="nextLevel()">
                <div v-for="shuffle in colorSelected">
                  <div class="rounded-full z-10" v-bind:class="scaling()" :class="shuffle"
                    @click="checkAnswer(shuffle)"></div>
                </div>
              </div>
            </div>
            <div class="flex justify-center">
              <button class="bg-yellow-500 h-10 w-40 rounded-xl mx-3 mt-10 text-white" @click="setMain()">Main
                menu</button>
              <button class="bg-yellow-500 h-10 w-40 rounded-xl mx-3 mt-10 text-white" @click="resetGame()">Restart
                game</button>
            </div>
          </div>
          <div class="basis-1/2 text-white">
          </div>
        </div>
      </div>
    </div>

    <!-- congratulation page -->
    <div :hidden="!gameEnd">
      <div class="flex flex-col">
        <img src="./Icon/Logo.png" alt="logo" width="200" class="m-4">
        <div class="flex justify-center">
          <div style="width:500px;height: 350px;" class="bg-slate-600 rounded-3xl mt-20 mx-24 flex flex-col">
            <p class="text-yellow-400 text-4xl font-bold text-center mt-14">Congratulations!</p>
            <p class="text-white font-bold text-center mt-14 text-3xl">{{ input== '' ? 'Guest' : input }}</p>
            <p class="text-white font-bold text-center mt-14 text-3xl">{{ score + ' Point!'}}</p>
          </div>
        </div>
        <div class="flex justify-center">
          <button class="text-white bg-yellow-500 rounded-lg h-10 w-40 mx-3 mt-7" @click="setMain()">Main menu</button>
          <button class="text-white bg-yellow-500 rounded-lg h-10 w-40 mx-3 mt-7" @click="resetGame()">Restart game</button>
        </div>

        <!-- firework -->
        <div class="m-0 p-0 h-100vh flex justify-center">
          <div class="firework" id="firework1">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework2">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework3">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework4">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework5">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework6">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework7">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework8">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework9">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
          <div class="firework" id="firework10">
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
            <div class="explosion"></div>
          </div>
        </div>
      </div>
    </div>



  </body>
</template>
 
<style scoped>
.firework {
  position: absolute;
}

.explosion {
  position: absolute;
  left: -2px;
  bottom: 0;
  width: 4px;
  height: 80px;
  transform-origin: 50% 100%;
  /* background-color: rgba(0,0,0,.2); */
  overflow: hidden;
}

.explosion:nth-child(1) {
  transform: rotate(0deg) translateY(-15px);
}

.explosion:nth-child(2) {
  transform: rotate(30deg) translateY(-15px);
}

.explosion:nth-child(3) {
  transform: rotate(60deg) translateY(-15px);
}

.explosion:nth-child(4) {
  transform: rotate(90deg) translateY(-15px);
}

.explosion:nth-child(5) {
  transform: rotate(120deg) translateY(-15px);
}

.explosion:nth-child(6) {
  transform: rotate(150deg) translateY(-15px);
}

.explosion:nth-child(7) {
  transform: rotate(180deg) translateY(-15px);
}

.explosion:nth-child(8) {
  transform: rotate(210deg) translateY(-15px);
}

.explosion:nth-child(9) {
  transform: rotate(240deg) translateY(-15px);
}

.explosion:nth-child(10) {
  transform: rotate(270deg) translateY(-15px);
}

.explosion:nth-child(11) {
  transform: rotate(300deg) translateY(-15px);
}

.explosion:nth-child(12) {
  transform: rotate(330deg) translateY(-15px);
}

.explosion::before {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  top: 100%;
  height: 40px;
  background-color: #f5cf52;
}

@keyframes explosion {
  0% {
    top: 100%;
  }

  33%,
  100% {
    top: -50%;
  }
}

#firework1 {
  left: 35%;
  top: 10%;
  transform: scale(1);
}

#firework1 .explosion::before {
  animation: explosion 2s ease-in-out infinite;
}

#firework2 {
  left: 75%;
  top: 35%;
  transform: scale(.7);
}

#firework2 .explosion::before {
  animation: explosion 2s .6s ease-in-out infinite;
}

#firework3 {
  left: 70%;
  top: 75%;
  transform: scale(.4);
}

#firework3 .explosion::before {
  animation: explosion 2s .4s ease-in-out infinite;
}

#firework4 {
  left: 30%;
  top: 75%;
  transform: scale(.8);
}

#firework4 .explosion::before {
  animation: explosion 2s .1s ease-in-out infinite;
}

#firework5 {
  left: 60%;
  top: 10%;
  transform: scale(.6);
}

#firework5 .explosion::before {
  animation: explosion 2s ease-in-out infinite;
}

#firework6 {
  left: 20%;
  top: 40%;
  transform: scale(.4);
}

#firework6 .explosion::before {
  animation: explosion 2s .1s ease-in-out infinite;
}

#firework7 {
  left: 10%;
  top: 90%;
  transform: scale(.6);
}

#firework7 .explosion::before {
  animation: explosion 2s .2s ease-in-out infinite;
}

#firework8 {
  left: 90%;
  top: 10%;
  transform: scale(.9);
}

#firework8 .explosion::before {
  animation: explosion 2s .2s ease-in-out infinite;
}

#firework9 {
  left: 85%;
  top: 85%;
  transform: scale(.9);
}

#firework9 .explosion::before {
  animation: explosion 2s ease-in-out infinite;
}

#firework10 {
  left: 15%;
  top: 10%;
  transform: scale(1);
}

#firework10 .explosion::before {
  animation: explosion 2s ease-in-out infinite;
}


</style>