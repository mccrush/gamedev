<template>
  <div class="container-fluid">
    <div class="row d-flex">
      <div
        v-for="(item, index) in arrayStart"
        :key="index"
        class="mc-height border col-4 text-center d-flex align-items-center justify-content-center"
        :class="'mc-bg-' + item.fg"
      >
        <div class="fs-1 fw-bold text-white text-shadow">{{ item.cn }}</div>
      </div>
    </div>
    <div class="row border-top pt-2 pb-2">
      <div class="col-12">
        <button class="btn btn-light shadow-sm" @click="setArrayStart">
          СТАРТ
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      arrayStart: [],
      arrayEnd: [],
      figures: ['tr', 'rc', 'rd'],
      numbers: ['1', '2', '3'],
      arrayStartTemp: [],
      figuresTemp: [],
      numbersTemp: []
    }
  },
  beforeMount() {
    this.resetData()
  },
  computed: {},
  methods: {
    setArrayStart() {
      this.resetData()
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
        if (!this.figuresTemp.includes(index)) {
          this.figuresTemp.push(index)
          j++
        }
      }

      let k = 0
      while (k < 3) {
        const index = this.getRND(2)
        if (!this.numbersTemp.includes(index)) {
          this.numbersTemp.push(index)
          k++
        }
      }

      for (let i = 0; i < 3; i++) {
        this.arrayStart[this.arrayStartTemp[i]] = {
          fg: this.figures[this.figuresTemp[i]],
          cn: this.numbers[this.numbersTemp[i]]
        }
      }

      console.log('this.arrayStart = ', this.arrayStart)

      // const index = this.getRND(5)
      // const figure = this.figures[this.getRND(2)]
      // const number = this.numbers[this.getRND(2)]
      // this.arrayStart[index] = { fg: figure, cn: number }
    },
    getRND(max) {
      return Math.floor(Math.random() * (max + 1))
    },
    resetData() {
      this.arrayStart = [
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' }
      ]
      this.arrayEnd = [
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' },
        { fg: '', cn: '' }
      ]
      this.arrayStartTemp = []
      this.figuresTemp = []
      this.numbersTemp = []
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