<script setup lang="ts">
const map = ref(new Array(100).fill(0))
const worm = ref(47)
const snake = ref([42, 43, 44])
const direction = ref(1)
const speed = ref(800)

function resetWorm(): void {
  const round = Math.round(Math.random() * 100)
  if (snake.value.includes(round))
    resetWorm()
  else
    worm.value = round
}
let timer: NodeJS.Timeout | undefined
function toWards(): void {
  const head: number = snake.value[snake.value.length - 1]
  // const tail = snake.value[0]

  const next: number = head + direction.value

  if (checkGame(next)) {
    clearInterval(timer)
    return
  }

  snake.value.push(next)
  if (next !== worm.value) {
    snake.value.shift()
    if (speed.value > 100)
      speed.value -= 50
  }
  else { resetWorm() }
}

function checkGame(next: number): boolean {
  if (snake.value.length === 100) {
    alert(`哇哦！ 😍${snake.value.length}厘米`)
    return true
  }

  if ((snake.value.includes(next))
    || (next < 0 || next > map.value.length)
    || (direction.value === 1 && next % 10 === 0)
    || (direction.value === -1 && next % 10 === 9)) {
    alert(`😍${snake.value.length}厘米`)
    return true
  }
  return false
}

function up(): void {
  if (direction.value === 10)
    return

  direction.value = -10
}
function left(): void {
  if (direction.value === 1)
    return
  direction.value = -1
}
function down(): void {
  if (direction.value === -10)
    return
  direction.value = 10
}
function right(): void {
  if (direction.value === -1)
    return
  direction.value = 1
}

function reset() {
  map.value = new Array(100).fill(0)
  worm.value = 47
  snake.value = [42, 43, 44]
  direction.value = 1
  speed.value = 800
  timer = setInterval(() => {
    toWards()
  }, speed.value)
}

onKeyStroke(['w', 'W', 'ArrowUp'], () => {
  up()
})
onKeyStroke(['s', 'S', 'ArrowDown'], () => {
  down()
})
onKeyStroke(['a', 'A', 'ArrowLeft'], () => {
  left()
})
onKeyStroke(['d', 'D', 'ArrowRight'], () => {
  right()
})
</script>

<template>
  <div flex="~" justify-center items-center flex-col gap-5>
    <div text-2xl text-gray>
      了子的贪吃蛇
    </div>
    <div>
      <button btn @click="reset">
        新的游戏
      </button>
    </div>
    <div flex="~" justify-center items-center w-102 flex-wrap border-2>
      <div v-for="(item, index) in map" :key="item + index"
        :class="[worm === index ? 'bg-gray' : '', snake.includes(index) ? 'bg-black' : '']" w-10 h-10 flex="~"
        justify-center items-center />
    </div>
    <div>
      <div>
        <button text-10 @click="up">
          ⬆️
        </button>
      </div>
      <div>
        <button text-10 @click="left">
          ⬅️
        </button>
        <button text-10 @click="down">
          ⬇️
        </button>
        <button text-10 @click="right">
          ➡️
        </button>
      </div>
    </div>
  </div>
</template>
