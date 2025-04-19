<template>
  <div class="damage-calc">
    <p>行動：{{ name }}</p>
    <p>最大ダメージ：{{ max }}</p>
    <p>最小ダメージ：{{ min }}</p>
  </div>
</template>

<script>
export default {
  name: 'DamageCalc',
  props: {
    atk: {
      type: Number,
      required: true
    },
    def: {
      type: Number,
      required: true
    },
    name: {
      type: String,
      required: true
    },
    magnification: {
      type: Number,
      required: true
    },
    isZone: {
      type: Boolean,
      default: false
    },
    allyBuff: {
      type: String,
      default: '0'
    },
    enemyBuff: {
      type: String,
      default: '0'
    }
  },
  computed: {
    zoneMagnification() {
      return this.isZone ? 1.2 : 1.0
    },
    allyBuffMagnification() {
      return {
        '-2': 0.25,
        '-1': 0.5,
        '0': 1.0,
        '+1': 1.5,
        '+2': 2.0
      }
    },
    enemyBuffMagnification() {
      return {
        '-2': 0.5,
        '-1': 0.75,
        '0': 1.0,
        '+1': 1.25,
        '+2': 1.5
      }
    },
    base() {
      return (
        (this.atk / 2) *
          this.enemyBuffMagnification[this.enemyBuff] *
          this.zoneMagnification -
        (this.def / 4) * this.allyBuffMagnification[this.allyBuff]
      )
    },
    range() {
      return this.base / 16 + 1
    },
    max() {
      const damage = Math.floor((this.base + this.range) * this.magnification)
      return damage > 0 ? damage : 1
    },
    min() {
      const damage = Math.floor((this.base - this.range) * this.magnification)
      return damage > 0 ? damage : 0
    }
  }
}
</script>

<style scoped>
.damage-calc p {
  margin: 0;
}
</style>
