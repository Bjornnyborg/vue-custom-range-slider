<template>
  <div class="slider" v-if="sliderValues.length">
    <div class="slider__wrapper">
      <div :style="{ left: position }" class="slider__label">{{ sliderValues[amount-1].label }}</div>
      <div class="slider__track" :class="{'slider__track--rectangular': !raising}">
        <div
          v-if="raising"
          :style="{ 'border-left-width': sliderWidth + 'px' }"
          class="slider__track-top"
        />
        <div
          v-if="raising"
          :style="{ 'border-right-width': sliderWidth + 'px' }"
          class="slider__track-bottom"
        />
      </div>
      <input
        ref="slider"
        :value="amount"
        :max="sliderValues.length"
        class="slider__input"
        type="range"
        min="1"
        @input="update"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      required: false,
      default: ""
    },
    values: {
      type: Array,
      required: false,
      default: () => []
    },
    min: {
      type: String,
      required: false,
      default: "0"
    },
    max: {
      type: String,
      required: false,
      default: "100"
    },
    raising: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data() {
    return {
      sliderWidth: 0,
      sliderValues: []
    };
  },
  computed: {
    amount() {
      let index = 0;

      this.sliderValues.map((item, i) => {
        if (item.value === this.value) {
          index = i;
        }
        return true;
      });

      return index + 1;
    },
    position() {
      const val = this.amount - 1;

      // Measure width of slider element. Adjust by 20 to account for thumbsize
      const width = this.sliderWidth - 20;

      // Calculate percentage between left and right of input
      const min = 0;
      const max = this.sliderValues.length - 1;
      const percent = (val - min) / (max - min);

      // Janky value to get pointer to line up better
      const offset = -2;

      const position = width * percent + offset;

      return `${position}px`;
    }
  },
  mounted() {
    if (this.values.length) {
      this.sliderValues = this.values;
    } else {
      var start = parseInt(this.min);
      var end = parseInt(this.max);

      while (start <= end) {
        this.sliderValues.push({
          label: start.toString(),
          value: start.toString()
        });
        start++;
      }
    }

    this.$nextTick(() => {
      this.sliderWidth = this.$refs.slider.clientWidth;
    });
  },
  methods: {
    update() {
      this.$emit("input", this.sliderValues[this.$refs.slider.value - 1].value);
    }
  }
};
</script>

<style lang="scss">
@import "../../public/vue-range-slider.scss";
</style>
