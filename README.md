# vue-custom-range-slider

This is a simple vue-range slider, that supports custom values, labels and more.
The component is based on use with v-model, the value is always a string, for supporting custom values.

## Demo

See the slider in action, in this [codesandbox](https://codesandbox.io/embed/vue-template-glwlb)

## Installation

### NPM

```bash
npm install --save vue-custom-range-slider
```

### Yarn

```bash
yarn add vue-custom-range-slider
```

## Usage

### Basic Usage

Import the vue-custom-rangeslider component, and start using it.

```html
<template>
  <div>
    <slider
      :values="sliderValues"
      min="0"
      max="100"
      raising
      v-model="slider"
    ></slider>
    <!-- remember to set v-model -->
    {{ slider }}
  </div>
</template>

<script>
  import Slider from "vue-custom-range-slider";
  // import the styling, css or scss
  import "vue-custom-range-slider/dist/vue-custom-range-slider.css";

  export default {
    components: {
      Slider
    },
    data() {
      return {
        slider: "0",
        sliderValues: [
          {
            label: "Not at all",
            value: "0"
          },
          {
            label: "A tiny bit",
            value: "1"
          },
          {
            label: "Its ok",
            value: "2"
          },
          {
            label: "Its very good",
            value: "3"
          },
          {
            label: "Its AMAZING!",
            value: "4"
          }
        ]
      };
    }
  };
</script>
```

Properties:

- `v-model` - name of the slider input.
- `min` - minimum value.
- `max` - maximum value.
- `values` - use a custom value object (overrides min/max).
- `raising` - set if you want the slider shape to be "raising" (like: ◁).

### Overwrite styling variables

You can easily modify the look of the slider, by overriding the variables in the scss file.

```sass
// override variables like this:
$label-color: red;

// import the styling,
@import "vue-custom-range-slider/dist/vue-range-slider.scss";

```

All variables can be found in [the scss file](dist/vue-range-slider.scss)

## License

MIT
