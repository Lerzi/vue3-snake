<script setup lang="ts">
enum TYPE_COLOR {
  DEFAULT = '',
  WORM = 'bg-gray',
  SNAKE = 'bg-dark',
}

const snackMap = reactive(new Array(50).fill('').map(() => new Array(50).fill('')))

const worm = ref({ r: 25, c: 30 })
const snake = ref(
  [
    { r: 25, c: 20 },
    { r: 25, c: 21 },
    { r: 25, c: 22 },
    { r: 25, c: 23 },
    { r: 25, c: 24 },
  ],
)

function getType(row: number, col: number) {
  if (worm.value.r === row && worm.value.c === col)
    return TYPE_COLOR.WORM

  if (snake.value.some(item => (item.r === row && item.c === col)))
    return TYPE_COLOR.SNAKE

  return TYPE_COLOR.DEFAULT
}
// const direction = ref(1)

// function checkGame(next: number): boolean {
//   if (snake.value.length === 100) {
//     alert(`哇哦！ 😍${snake.value.length}厘米`)
//     return true
//   }

//   if ((snake.value.includes(next))
//     || (next < 0 || next > map.value.length)
//     || (direction.value === 1 && next % 10 === 0)
//     || (direction.value === -1 && next % 10 === 9)) {
//     alert(`😍${snake.value.length}厘米`)
//     return true
//   }
//   return false
// }

// console.log(checkGame)

function up(): void {

}
function left(): void {

}
function down(): void {

}
function right(): void {

}

function reset() {

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
    <div border-2>
      <div v-for="(rItem, rIndex) in snackMap" :key="rIndex" flex="~" justify-center items-center>
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
