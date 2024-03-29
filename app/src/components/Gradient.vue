<script>
// Inspired by:  https://github.com/alexmilde/vue-gradient
// TODO set this a TypeScript type
const Color = require("color");

export default {
  name: "Gradient",
  props: {
    colors: {
      type: Array,
      required: true,
    },
    blendMode: {
      type: String,
      required: false,
      default: "normal",
    },
    gradientSize: {
      type: String,
      required: false,
      default: "farthest-corner",
    },
    middleColorOpaquer: {
      type: Number,
      required: false,
      default: -0.75,
    },
    showGradient: {
      type: Boolean,
      required: false,
      default: true,
    },
  },
  data() {
    return {
      supportedSpeeds: [0.3, 0.5, 1, 2],
      newColorsPushed: false,
      colorConfigurations: [this.$props.colors],
    };
  },
  watch: {
    colors: function (colors) {
      this.newColorsPushed = true;
      this.colorConfigurations.push(colors);
    },
  },
  methods: {
    createStyleForConfig(config) {
      let gradients = [];

      config.forEach((colorData) => {
        let colorStart = Color(colorData.color_start);
        let colorMiddle = Color(
          colorData.color_middle
            ? colorData.color_middle
            : colorData.color_start
        );
        if (!colorData.color_middle) {
          colorMiddle = colorMiddle.opaquer(this.middleColorOpaquer);
        }

        let colorStrStart = `rgba(${colorStart.red()},${colorStart.green()},${colorStart.blue()},${colorStart.alpha()})`;
        let colorStrMiddle = `rgba(${colorMiddle.red()},${colorMiddle.green()},${colorMiddle.blue()},${colorMiddle.alpha()})`;

        let gradientStr = `radial-gradient(circle ${this.gradientSize} at ${colorData.x}% ${colorData.y}%, ${colorStrStart}, ${colorStrMiddle}, transparent)`;
        gradients.push(gradientStr);
      });

      return {
        "background-image": gradients.join(","),
        "background-blend-mode": this.blendMode,
      };
    },
  },
};
</script>

<template>
  <div class="p-r">
    <transition name="fade">
      <div v-if="showGradient">
        <div
          v-for="(config, index) in colorConfigurations"
          class="p-a wh-full gradient"
          :key="config + index"
          :style="createStyleForConfig(config)"
        ></div>
      </div>
    </transition>
  </div>
</template>

<style scoped>
.p-a {
  position: absolute;
}

.p-r {
  position: relative;
}

.wh-full {
  width: 100%;
  height: 100%;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.15s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>