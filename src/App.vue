<template>
  <div id="app">
    <div class="app-memo">
      <h2>エマちゃんはかわいいα（DQ11物理被ダメージ計算のようなもの）</h2>
      <p>
        間違っている可能性は高いので利用は自己責任でお願いします。<br />
        味方が使える特技で敵が使ってくるものは基本的にダメージ倍率が不明です。<br />
        私は解析できない&解析情報を知らないため、実測値で見るしかなさそうです。<br />
        このくらいかなってのあれば教えてください。<br />
        調査とか手伝ってくれる人募集してます。<br />
      </p>
      <p>
        主Lv13守りの種1ユグノア+竜のうろこ+ちからのゆびわ守備力:104
        <button @click="vsArachtagon">適用</button>
      </p>
      <p>
        主Lv22守りの種1まほうの盾+しっぷう+シルメ守備力:139
        <button @click="vsTyriant">適用</button>
      </p>
    </div>
    <div class="app-input">
      <div class="input">
        <label>モンスター</label>
        <select v-model="monster">
          <option
            v-for="monster of monsters"
            :key="monster.no"
            :value="monster"
          >
            {{ monster.name }}
          </option>
        </select>
      </div>
      <div class="input">
        <label>守備力</label>
        <input type="number" v-model="def" />
      </div>
      <div class="input">
        <label>敵ゾーン状態</label>
        <input type="checkbox" v-model="isZone" />
      </div>
      <div class="input">
        <label>敵バフ</label>
        <select v-model="enemyBuff">
          <option v-for="value of buffValues" :key="value" :value="value">
            {{ value }}
          </option>
        </select>
      </div>
      <div class="input">
        <label>味方バフ</label>
        <select v-model="allyBuff">
          <option v-for="value of buffValues" :key="value" :value="value">
            {{ value }}
          </option>
        </select>
      </div>
    </div>
    <div class="app-results" v-if="monster && def">
      <damage-calc
        class="results"
        v-for="number of monster.skillNumbers"
        :key="number"
        v-bind="calculationParameters(number)"
      />
    </div>
  </div>
</template>

<script>
import { MONSTERS } from '@/assets/data/monsters'
import { SKILLS } from '@/assets/data/skills'
import DamageCalc from '@/components/DamageCalc'

export default {
  name: 'App',
  components: { DamageCalc },
  data() {
    return {
      monster: null,
      def: 104,
      isZone: false,
      enemyBuff: '0',
      allyBuff: '0'
    }
  },
  computed: {
    monsters() {
      return MONSTERS
    },
    buffValues() {
      return ['-2', '-1', '0', '+1', '+2']
    }
  },
  methods: {
    calculationParameters(number) {
      return {
        ...SKILLS.find(s => s.no === number),
        atk: this.monster.atk,
        def: Number(this.def),
        isZone: this.isZone,
        enemyBuff: this.enemyBuff,
        allyBuff: this.allyBuff
      }
    },
    vsArachtagon() {
      this.def = 104
      this.monster = this.monsters.find(m => m.no === 16)
    },
    vsTyriant() {
      this.def = 139
      this.monster = this.monsters.find(m => m.no === 23)
    }
  }
}
</script>

<style>
.app-memo {
  margin-bottom: 12px;
}

.app-memo p {
  margin: 0;
}

.app-input {
  display: flex;
}

.input select {
  padding-bottom: 4px;
}

.results {
  margin-top: 16px;
}
</style>
