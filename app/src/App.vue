
<template>
  <v-app id="inspire">
    <v-toolbar-title
      class="d-flex text-h2 justify-space-around align-center my-10"
    >
      JdubHub</v-toolbar-title
    >
    <v-toolbar-title
    class="d-flex text-h5 justify-space-around align-center my-2"
    >The collective works of software by Justin Wesley</v-toolbar-title>
    <v-main>
      <v-container>
        <v-row>
          <v-col v-for="site in sites" :key="site.id" cols="4">
            <v-card
              @click="mouseClick(site.domain)"
              v-on:mouseover="mouseOver(site.id)"
              v-on:mouseleave="mouseLeave"
            >
              <v-card-text
                class="d-flex text-h4 fill-height justify-center align-center"
                style="position: absolute"
                >{{
                  mouseOverCardID === site.id ? site.domain : site.name
                }}</v-card-text
              >
              <v-card-text
                class="
                  d-flex
                  text-h8
                  font-italic
                  fill-height
                  justify-center
                  align-center
                "
                style="position: absolute; top: 2.5em"
              >
                {{
                  mouseOverCardID === site.id ? "" : site.description
                }}</v-card-text
              >
              <div class="d-flex fill-height justify-center align-center">
                <Gradient
                  class="wh-full"
                  :colors="colors"
                  :showGradient="showGradient && site.id === siteChosenForColor"
                />
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";
import Gradient from "./components/Gradient.vue";

export default Vue.extend({
  name: "App",

  components: {
    Gradient,
  },

  mounted: function () {
    this.colorChanger();
  },

  methods: {
    colorChanger: function () {
      setInterval(() => {
        this.colors = this.shuffle(this.colors);
        if (this.showGradient) {
          this.showGradient = false;
        } else if (this.mouseOverCardID !== -1) {
          this.showGradient = true;
          this.siteChosenForColor = this.mouseOverCardID;
        } else if (Math.floor(Math.random() * 3) === 0) {
          this.showGradient = true;
        } else {
          this.showGradient = false;
        }
      }, 500);
    },

    shuffle: function (
      array: Array<{
        x: number;
        y: number;
        color_start: string;
        color_middle: string;
      }>
    ) {
      const optionsIndex = Math.floor(Math.random() * this.colorOptions.length);
      this.siteChosenForColor = Math.floor(Math.random() * this.sites.length);
      const randomIndex = Math.floor(Math.random() * array.length);
      const randomValueX = Math.floor(Math.random() * 100);
      const randomValueY = Math.floor(Math.random() * 100);
      array[randomIndex] = {
        x: randomValueX,
        y: randomValueY,
        color_start: this.colorOptions[optionsIndex],
        color_middle: this.colorOptions[optionsIndex] + "59", // 35% opacity
      };

      return array;
    },
    mouseOver: function (siteID: number) {
      this.mouseOverCardID = siteID;
    },
    mouseLeave: function () {
      this.mouseOverCardID = -1;
    },
    mouseClick: function (domain: string) {
      window.open(`https://${domain}`, "_blank");
    },
  },

  data: () => ({
    showGradient: true,
    sites: [
      {
        id: 0,
        name: "Github Repositories",
        domain: "github.com/Jdubedition",
        description: "publicly available repositories",
      },
      {
        id: 1,
        name: "Bountiful Bytes",
        domain: "bountifulbytes.com",
        description: "journey through the world of building software",
      },
      {
        id: 2,
        name: "DApp One Word At A Time",
        domain: "dapp-owaat.jdubedition.com",
        description: "decentralized application of one word at a time",
      },
      {
        id: 3,
        name: "Molereum Faucet",
        domain: "faucet.jdubedition.com",
        description: "faucet to receive MOLE for Molereum network",
      },
      {
        id: 4,
        name: "Pearl Performance Chiropractic",
        domain: "pearlperformancechiropractic.com",
        description: "professional chiropractic website",
      },
    ],
    siteChosenForColor: 0,
    colorOptions: [
      "#ffff00", // yellow
      "#00ff00", // green
      "#ff0000", // red
      "#00ffff", // cyan
      "#ff00ff", // magenta
      "#FF6037", // orange
      "#FFB300", // amber
      "#FFCC33", // sunglow
      "#CCFF00", // lime
      "#00CCFF", // sky blue
      "#50BFE6", // blizzard blue
    ],
    colors: [
      {
        x: 0,
        y: 0,
        color_start: "#ffff00ff",
        color_middle: "#ffff0059",
      },
    ],
    mouseOverCardID: -1,
  }),
});
</script>

<style>
.v-card {
  height: 200px;
}
</style>