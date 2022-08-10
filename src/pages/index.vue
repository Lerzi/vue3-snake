<script setup lang="ts">
import type { Ref } from 'vue'
enum TYPE_COLOR {
  DEFAULT = '',
  WORM = 'bg-gray',
  SNAKE = 'bg-dark',
}

interface Cell {
  r: number
  c: number
}

const snakeMap = reactive(new Array(50).fill('').map(() => new Array(50).fill('')))

type Direction = 'up' | 'down' | 'left' | 'right'
const direction: Ref<Direction> = ref('right')

const worm: Ref<Cell> = ref({ r: NaN, c: NaN })

function resetWorm(): void {
  const round: Cell = { r: Math.round(Math.random() * 50), c: Math.round(Math.random() * 50) }

  if (isSnake(round.r, round.c))
    resetWorm()
  else
    worm.value = round
}

const snake: Ref<Cell[]> = ref(
  [
    { r: NaN, c: NaN },
  ],
)

function resetSnake() {
  snake.value = [
    { r: 25, c: 20 },
    { r: 25, c: 21 },
    { r: 25, c: 22 },
    { r: 25, c: 23 },
    { r: 25, c: 24 },
  ]
}
resetSnake()

let timer: NodeJS.Timeout | undefined
function getType(row: number, col: number) {
  if (isWorm(row, col))
    return TYPE_COLOR.WORM

  if (isSnake(row, col))
    return TYPE_COLOR.SNAKE

  return TYPE_COLOR.DEFAULT
}

function creep(): void {
  // 开始运行

  const lastItem = snake.value[snake.value.length - 1]
  let next: Cell = { r: NaN, c: NaN }
  if (direction.value === 'up')
    next = { r: (lastItem.r - 1), c: lastItem.c }
  if (direction.value === 'down')
    next = { r: (lastItem.r + 1), c: lastItem.c }
  if (direction.value === 'left')
    next = { r: lastItem.r, c: (lastItem.c - 1) }
  if (direction.value === 'right')
    next = { r: lastItem.r, c: (lastItem.c + 1) }
  if (checkGame(next)) {
    clearInterval(timer)
    return
  }
  if (isWorm(next.r, next.c))
    resetWorm()
  else
    snake.value.shift()

  snake.value.push(next)
}

function isWorm(row: number, col: number) {
  return worm.value.r === row && worm.value.c === col
}

function isSnake(row: number, col: number) {
  return snake.value.some(item => (item.r === row && item.c === col))
}

// const direction = ref(1)

function checkGame(next: Cell): boolean {
  if (isSnake(next.r, next.c) || next.r > 50 || next.r < 0 || next.c < 0 || next.c > 50) {
    alert(`${snake.value.length}cm`)
    return true
  }
  return false
}

function up(): void {
  if (!timer)
    reset()

  if (direction.value !== 'down')
    direction.value = 'up'
}
function left(): void {
  if (!timer)
    reset()
  if (direction.value !== 'right')
    direction.value = 'left'
}
function down(): void {
  if (!timer)
    reset()
  if (direction.value !== 'up')
    direction.value = 'down'
}
function right(): void {
  if (!timer)
    reset()
  if (direction.value !== 'left')
    direction.value = 'right'
}

function reset() {
  resetWorm()
  resetSnake()
  direction.value = 'right'
  timer = setInterval(() => {
    creep()
  }, 80)
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
        开始
      </button>
    </div>
    <div border-2>
      <div v-for="(rItem, rIndex) in snakeMap" :key="rIndex" flex="~" justify-center items-center>
        <div v-for="(cItem, cIndex) in rItem" :key="cItem + cIndex" w-2 h-2 :class="getType(rIndex, cIndex)" />
      </div>
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
