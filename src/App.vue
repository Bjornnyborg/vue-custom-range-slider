<template>
  <div id="app">
    <div class="wrapper" v-if="sliderValues && sliderValues.length">
      <custom-slider min="10" max="50" step="1" :values="sliderValues" raising v-model="slider" />
    </div>
    {{ slider }}

    <button @click="changeValue()">Change value to 2nd option</button>

    <button @click="changeOptions()">Change options</button>
  </div>
</template>

<script>
import CustomSlider from "./components/CustomRangeSlider.vue";

export default {
  name: "app",
  components: {
    CustomSlider
  },
  data() {
    return {
      changed: false,
      slider: "a",
      sliderValues: null,
      sliderValues1: [
        {
          label: "Not at all",
          value: "a"
        },
        {
          label: "A tiny bit",
          value: "b"
        },
        {
          label: "Its ok",
          value: "c"
        },
        {
          label: "Its very good",
          value: "d"
        },
        {
          label: "Its AMAZING!",
          value: "e"
        }
      ],
      sliderValues2: [
        {
          label: "Something else",
          value: "1"
        },
        {
          label: "Something else else",
          value: "2"
        },
        {
          label: "Comp",
          value: "3"
        }
      ]
    };
  },
  mounted() {
    this.sliderValues = this.sliderValues1;
  },
  methods: {
    changeOptions() {
      this.sliderValues = this.changed ? this.sliderValues1 : this.sliderValues2;
      this.changed = !this.changed;
    },
    changeValue() {
      this.slider = this.sliderValues[1].value;
    }
  }
};
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  background: #fff;
  padding: 0 50px;
  height: 100vh;
  margin: 0;
}

.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
