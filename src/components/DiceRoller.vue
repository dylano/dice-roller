<template>
  <div>
    <div class="dice">
      <div>
        <span class="roll-total" :class="{ highlight: diceSum > 0 }" @click="reset()">
          {{ diceSum }}
        </span>
      </div>
      <div class="die" v-for="idx in numDice" :key="idx">
        <button
          class="die-btn"
          @click="roll(idx)"
          @mouseenter="hover(true, idx)"
          @mouseleave="hover(false, idx)"
        >
          <img class="die-img" :src="imgPath(idx)" :alt="`d${sides}-${idx}`" />
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
      default: 4
    }
  },
  data() {
    return {
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
    reset() {
      this.rolls = [0];
      this.activeDice = 0;
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

.highlight {
  color: yellow;
  background-color: black;
  border-radius: 50%;
}

.roll-total {
  margin: 2rem 5px;
  display: flex;
  justify-content: center;
  cursor: pointer;
  font-size: 1.5rem;
  font-weight: 800;
  transition: all 200ms ease-in-out;
}
</style>
