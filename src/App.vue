<template>
  <div class="container-fluid">
    <div v-if="mod === 'start'" class="row d-flex">
      <div
        v-for="(item, index) in arrayStart"
        :key="index"
        class="mc-height col-4 text-center d-flex align-items-center justify-content-center"
        :class="'mc-bg-' + item.fg"
      >
        <div class="fs-1 fw-bold text-white text-shadow">{{ item.cn }}</div>
      </div>
    </div>
    <div v-if="mod === 'end'" class="row d-flex">
      <div
        v-for="(item, index) in arrayEnd"
        :key="index"
        class="mc-height col-4 text-center d-flex align-items-center justify-content-center"
        :class="'mc-bg-' + item.fg"
        @click="checkSelect(index, item.fg, $event)"
      ></div>
    </div>
    <div class="row pt-2 pb-2">
      <div class="col-6">
        <button class="btn btn-light shadow-sm fw-bold" @click="setArrayStart">
          СТАРТ
        </button>
        <!-- <button
          class="btn btn-light shadow-sm fw-bold ms-2"
          @click="setArrayEnd"
        >
          ЧЕК
        </button> -->
        <button class="btn btn-light shadow-sm fw-bold ms-2" @click="stopGame">
          СТОП
        </button>
      </div>
      <div class="col-6 d-flex align-items-center justify-content-end">
        <span>Таймер с.</span>
        <input
          class="form-control form-control-sm w-25 ms-2"
          type="number"
          v-model="timer"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      timer: 3,
      startGame: false,
      mod: 'start',
      clickCounter: 0,
      arrayStart: [],
      arrayEnd: [],
      figures: ['tr', 'rc', 'rd'],
      numbers: ['1', '2', '3'],
      arrayStartTemp: [],
      figuresStartTemp: [],
      numbersStartTemp: [],
      arrayEndTemp: [],
      figuresEndTemp: [],
      numbersEndTemp: []
    }
  },
  beforeMount() {
    this.resetStartData()
    this.resetEndData()
  },
  computed: {},
  methods: {
    stopGame() {
      this.startGame = false
      this.resetStartData()
      this.resetEndData()
    },
    checkSelect(index, figure, event) {
      //console.log('index = ', index)
      //console.log('figure = ', figure)
      //console.log('e = ', event.target)

      if (this.clickCounter === 3 || !this.startGame) return

      this.clickCounter++

      if (figure) {
        const targetClickNumber = this.arrayStart.find(
          item => item.fg === figure
        ).cn

        //console.log('targetClickNumber = ', targetClickNumber)
        if (this.clickCounter == targetClickNumber) {
          event.target.classList.remove('bg-danger')
          event.target.classList.add('bg-success')

          if (this.clickCounter === 3 && this.startGame) {
            setTimeout(() => {
              this.setArrayStart()
            }, this.timer * 1000)
          }
        } else {
          this.clickCounter--
          event.target.classList.add('bg-danger')
        }
      } else {
        this.clickCounter--
        event.target.classList.add('bg-danger')
      }

      //console.log('this.clickCounter = ', this.clickCounter)
    },
    setArrayStart() {
      this.startGame = true
      this.resetStartData()
      let i = 0
      while (i < 3) {
        const index = this.getRND(5)
        if (!this.arrayStartTemp.includes(index)) {
          this.arrayStartTemp.push(index)
          i++
        }
      }

      let j = 0
      while (j < 3) {
        const index = this.getRND(2)
        if (!this.figuresStartTemp.includes(index)) {
          this.figuresStartTemp.push(index)
          j++
        }
      }

      let k = 0
      while (k < 3) {
        const index = this.getRND(2)
        if (!this.numbersStartTemp.includes(index)) {
          this.numbersStartTemp.push(index)
          k++
        }
      }

      for (let i = 0; i < 3; i++) {
        this.arrayStart[this.arrayStartTemp[i]] = {
          fg: this.figures[this.figuresStartTemp[i]],
          cn: this.numbers[this.numbersStartTemp[i]]
        }
      }

      setTimeout(() => {
        this.setArrayEnd()
      }, this.timer * 1000)

      //console.log('this.arrayStart = ', this.arrayStart)
    },
    setArrayEnd() {
      this.resetEndData()
      let i = 0
      while (i < 3) {
        const index = this.getRND(5)
        if (!this.arrayEndTemp.includes(index)) {
          this.arrayEndTemp.push(index)
          i++
        }
      }

      let j = 0
      while (j < 3) {
        const index = this.getRND(2)
        if (!this.figuresEndTemp.includes(index)) {
          this.figuresEndTemp.push(index)
          j++
        }
      }

      let k = 0
      while (k < 3) {
        const index = this.getRND(2)
        if (!this.numbersEndTemp.includes(index)) {
          this.numbersEndTemp.push(index)
          k++
        }
      }

      for (let i = 0; i < 3; i++) {
        this.arrayEnd[this.arrayEndTemp[i]] = {
          fg: this.figures[this.figuresEndTemp[i]],
          cn: this.numbers[this.numbersEndTemp[i]]
        }
      }

      //console.log('this.arrayEnd = ', this.arrayEnd)
    },
    getRND(max) {
      return Math.floor(Math.random() * (max + 1))
    },
    resetStartData() {
      this.mod = 'start'
      this.arrayStart = [
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' }
      ]
      this.arrayStartTemp = []
      this.figuresStartTemp = []
      this.numbersStartTemp = []
    },
    resetEndData() {
      this.mod = 'end'
      this.clickCounter = 0
      this.arrayEnd = [
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' }
      ]
      this.arrayEndTemp = []
      this.figuresEndTemp = []
      this.numbersEndTemp = []
    }
  }
}
</script>

<style lang="scss">
@import './scss/common';

.mc-height {
  height: calc((100svh - 56px) / 2);
}

.mc-bg-tr {
  background: url(./images/figures/tr.png) center center no-repeat;
  background-size: contain;
}
.mc-bg-rc {
  background: url(./images/figures/rc.png) center top no-repeat;
  background-size: contain;
}
.mc-bg-rd {
  background: url(./images/figures/rd.png) center top no-repeat;
  background-size: contain;
}
</style>