<template>
  <div>
    <div class="dice">
      <div>
        <span class="roll-total">{{ diceSum }}</span>
      </div>
      <div class="die" v-for="idx in numDice" :key="idx">
        <button
          class="die-btn"
          @click="roll(idx)"
          @mouseenter="hover(true, idx)"
          @mouseleave="hover(false, idx)"
        >
          <img class="die-img" :src="imgPath(idx)" :alt="`d${sides}`" />
        </button>
        <span class="die-result">{{ val(idx) }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    sides: {
      type: Number,
      default: 6
    },
    numDice: {
      type: Number,
      default: 5
    }
  },
  data() {
    return {
      resultStr: '',
      rolls: [0],
      activeDice: 0
    };
  },
  computed: {
    diceSum() {
      const sum = this.rolls.reduce((total, roll) => total + roll);
      return sum ? sum : `d${this.sides}`;
    }
  },
  methods: {
    roll(numDice) {
      this.activeDice = numDice;
      this.rolls = new Array(this.numDice).fill(0);
      for (let i = 0; i < numDice; i++) {
        this.rolls[i] = Math.floor(Math.random() * this.sides) + 1;
      }
    },
    val(index) {
      return this.rolls[index - 1] ? this.rolls[index - 1] : '';
    },
    imgPath(idx) {
      const base = `d${this.sides}`;
      const suffix = idx <= this.activeDice ? '' : '-disabled';
      return require(`../assets/${base}${suffix}.svg`);
    },
    hover(isHovering, idx) {
      if (isHovering) {
        this.activeDice = idx;
      } else {
        let count = 0;
        this.rolls.forEach(roll => {
          if (roll > 0) {
            count++;
          }
        });
        this.activeDice = count;
      }
    }
  }
};
</script>

<style scoped>
.dice {
  display: grid;
  grid-template-columns: 50px repeat(auto-fit, minmax(0, 1fr));
}

.spin {
  transform: rotate(100);
}

.die {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1rem;
}

.die-img {
  height: 4rem;
}

.die-btn {
  padding: 0;
  border: 0;
  margin: 0.5rem;
  background-color: #fff;
  cursor: pointer;
}

.die-result {
  font-size: 1.5rem;
  height: 1.5rem;
}

.roll-total {
  font-size: 1.5rem;
  font-weight: 800;
  display: flex;
  justify-content: center;
  padding-top: 2rem;
}
</style>
