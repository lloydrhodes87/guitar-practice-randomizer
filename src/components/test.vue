<template>
  <div class="hello">
    <div>
      <p>Key: {{key}}</p>
      <p>Tonality: {{tonality}}</p>
      <p>Technique: {{technique}}</p>
      <p>Area: {{area}}</p>
      <button id="practiceClick" @click="generator">Practice</button>
      <button @click=" autoGenerate()" :disabled="autoDisable">Auto</button>
      <button @click="clear">Clear</button>
      <label for="bpm">BPM</label>
      <input type="text" v-model="bpm" @change="logBPM" id="bpm" placeholder="bpm">
      <p v-show="errorInvalid !== ''">{{errorInvalid}}</p>
      <p v-show="inRange === false">{{errorRange}}</p>
      <button>3/4</button>
      <button>4/4</button>
    </div>
  </div>
</template>

<script>
import { clearInterval } from "timers";
export default {
  data() {
    return {
      keys: [
        "A",
        "A#/Bb",
        "B",
        "C",
        "C#/Db",
        "D",
        "D#/Eb",
        "E",
        "F",
        "F#/Gb",
        "G",
        "G#/Ab"
      ],
      tonalities: ["Major", "Minor"],
      techniques: ["Chords", "Scales"],
      areas: ["Open", "3", "5", "7", "10"],
      area: "",
      key: "",
      tonality: "",
      technique: "",
      interval: "",
      bpm: "",
      errorInvalid: "",
      inRange: false,
      errorRange: "",
      millisecondValue: "",
      autoDisable: false,
      audio: require("../assets/click2.mp3")
    };
  },

  methods: {
    generator() {
      this.key = this.keys[Math.floor(Math.random() * this.keys.length)];
      this.tonality = this.tonalities[
        Math.floor(Math.random() * this.tonalities.length)
      ];
      this.technique = this.techniques[
        Math.floor(Math.random() * this.techniques.length)
      ];
      this.area = this.areas[Math.floor(Math.random() * this.areas.length)];
    },
    playAudio() {
      const audio = new Audio(this.audio);
      audio.play();
    },
    autoGenerate() {
      this.generator();

      this.interval = setInterval(() => {}, this.millisecondValue);

      this.autoDisable = true;
    },
    autoGenerateTwo() {
      this.changeInterval = setInterval(() => {
        this.generator();
      }, this.millisecondValue * 4);
    },
    clear() {
      window.clearInterval(this.interval);
      window.clearInterval(this.changeInterval);
      this.key = "";
      this.tonality = "";
      this.technique = "";
      this.area = "";
      this.autoDisable = false;
    },
    logBPM() {
      this.bpm = Number(this.bpm);
      if (isNaN(this.bpm) === true) {
        this.errorInvalid = "Please enter a valid number";
        this.bpm = "";
      } else if (this.bpm < 40 || this.bpm >= 400) {
        this.bpm = "";
        this.inRange = false;
        this.errorRange = "Please enter a BPM value between 40 and 400";
      } else {
        this.millisecondValue = 60000 / this.bpm;
        this.bpm = this.bpm;
        this.inRange = true;
        this.errorInvalid = "";
        this.errorRange = "";
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
