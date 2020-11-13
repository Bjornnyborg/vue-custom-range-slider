<template>
  <div class="slider">
    <div class="slider__wrapper">
      <div v-if="!hideLabel" :style="{ left: position }" class="slider__label">{{ sliderLabel }}</div>
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
        v-model="sliderValue"
        :max="sliderMax"
        class="slider__input"
        type="range"
        :min="sliderMin"
        :step="step"
        @input="update"
        @change="change"
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
    step: {
      type: String,
      required: false,
      default: "1"
    },
    hideLabel: {
      type: Boolean,
      required: false,
      default: false
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
      sliderValues: [],
      sliderValue: null,
      sliderMax: null,
      sliderMin: null
    };
  },
  computed: {
    sliderLabel() {
      // If using custom values, the custom label is returned, otherwise the value is also the label
      return this.sliderValues.length
        ? this.sliderValues[this.sliderValue - 1].label
        : this.sliderValue;
    },
    sliderOutputValue() {
      // If using custom values, the custom value is returned, otherwise just the default value
      return this.sliderValues.length
        ? this.sliderValues[this.sliderValue - 1].value
        : this.sliderValue;
    },
    position() {
      const val = this.sliderValue;
      // Measure width of slider element. Adjust by 20 to account for thumbsize
      const width = this.sliderWidth - 20;

      // Calculate percentage between left and right of input
      const percent =
        (val - this.sliderMin) / (this.sliderMax - this.sliderMin);

      // Janky value to get pointer to line up better
      const offset = -2;

      const position = width * percent + offset;

      return `${position}px`;
    }
  },
  mounted() {
    this.changeValues();
    this.$nextTick(() => {
      this.resizeHandler();
    });
  },
  methods: {
    changeValues() {
      // Set local values, depending on use of custom or default
      if (this.values.length) {
        this.sliderValues = this.values;
        this.sliderMin = "1";
        this.sliderMax = this.sliderValues.length;

        // Find the corresponding custom value, and set the local sliderValue
        let index = 0;
        this.values.map((item, i) => {
          if (item.value === this.value) {
            index = i;
          }
          return true;
        });
        this.sliderValue = index + 1;
      } else {
        // In case of using default slider methods
        this.sliderMin = this.min;
        this.sliderMax = this.max;
        this.sliderValue = this.value;
      }
      this.update();
    },
    update() {
      this.$emit("input", this.sliderOutputValue);
    },
    change() {
      this.$emit("change", this.sliderOutputValue);
    },
    resizeHandler() {
      this.sliderWidth = this.$refs.slider.clientWidth;
    }
  },
  created() {
    window.addEventListener("resize", this.resizeHandler);
  },
  destroyed() {
    window.removeEventListener("resize", this.resizeHandler);
  },
  watch: {
    values: {
      immediate: true, 
      handler () {
        this.changeValues();
      }
    },
    value: {
      immediate: true, 
      handler () {
        this.changeValues();
      }
    }
  },
};
</script>

<style lang="scss">
@import "../../public/vue-custom-range-slider.scss";
</style>