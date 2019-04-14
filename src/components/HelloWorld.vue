<template>
  <div>
    <div class="options">
      <div class="col">
        <h2>Key</h2>
        <label for="A">
          A
          <input type="checkbox" id="A" v-model="notes" value="A  |   ">
        </label>
        <label for="A#/Bb">
          A#/Bb
          <input type="checkbox" id="A#/Bb" v-model="notes" value="A#/Bb  |   ">
        </label>
        <label for="B">
          B
          <input type="checkbox" id="B" v-model="notes" value="B  |   ">
        </label>
        <label for="C">
          C
          <input type="checkbox" id="C" v-model="notes" value="C  |   ">
        </label>
        <label for="C#/Db">
          C#/Db
          <input type="checkbox" id="C#/Db" v-model="notes" value="C#/Db  |   ">
        </label>
        <label for="D">
          D
          <input type="checkbox" id="D" v-model="notes" value="D  |   ">
        </label>
        <label for="D#/Eb">
          D#/Eb
          <input type="checkbox" id="D#/Eb" v-model="notes" value="D#/Eb  |   ">
        </label>
        <label for="E">
          E
          <input type="checkbox" id="E" v-model="notes" value="E  |   ">
        </label>
        <label for="F">
          F
          <input type="checkbox" id="F" v-model="notes" value="F  |   ">
        </label>
        <label for="F#/Gb">
          F#/Gb
          <input type="checkbox" id="F#/Gb" v-model="notes" value="F#/Gb  |   ">
        </label>
        <label for="G">
          G
          <input type="checkbox" id="G" v-model="notes" value="G  |   ">
        </label>
        <label for="G#/Ab">
          G#/Ab
          <input type="checkbox" id="G#/Ab" v-model="notes" value="G#/Ab  |   ">
        </label>
      </div>

      <div class="col">
        <h2>Technique</h2>
        <label for="Scales">
          Scales
          <input type="checkbox" id="Scales" v-model="techniques" value="Scales  |   ">
        </label>
        <label for="Chords">
          Chords
          <input type="checkbox" id="Chords" v-model="techniques" value="Chords  |   ">
        </label>

        <h2>Tonality</h2>
        <label for="Major">
          Major
          <input type="checkbox" id="Major" v-model="tonalities" value="Major  |   ">
        </label>
        <label for="Minor">
          Minor
          <input type="checkbox" id="Minor" v-model="tonalities" value="Minor  |   ">
        </label>
      </div>

      <div class="col">
        <h2>Area</h2>
        <label for="Root">
          Root
          <input type="checkbox" id="Root" v-model="areas" value="Open">
        </label>
        <label for="third">
          3rd Fret
          <input type="checkbox" id="third" v-model="areas" value="Third Fret">
        </label>
        <label for="fifth">
          5th Fret
          <input type="checkbox" id="fifth" v-model="areas" value="Fifth Fret">
        </label>
        <label for="seventh">
          7th Fret
          <input type="checkbox" id="seventh" v-model="areas" value="Seventh Fret">
        </label>
        <label for="tenth">
          10th Fret
          <input type="checkbox" id="tenth" v-model="areas" value="Tenth Fret">
        </label>
        <label for="root E1">
          Root on E
          <input type="checkbox" id="root E1" v-model="areas" value="Root on E 6th String">
        </label>
        <label for="root A">
          Root on A
          <input type="checkbox" id="root A" v-model="areas" value="Root on A">
        </label>
        <label for="root D">
          Root on D
          <input type="checkbox" id="root D" v-model="areas" value="Root on D">
        </label>
        <label for="root G">
          Root on G
          <input type="checkbox" id="root G" v-model="areas" value="Root  on G">
        </label>
        <label for="root B">
          Root on B
          <input type="checkbox" id="root B" v-model="areas" value="Root  on B">
        </label>
        <label for="root E2">
          Root on E
          <input
            type="checkbox"
            id="root E2"
            v-model="areas"
            value="Root  on E 1st String"
          >
        </label>
      </div>
    </div>
    <div class="bottom">
      <div>
        <label for="seconds">
          Change practice schedule every
          <input
            type="text"
            v-model="seconds"
            placeholder="seconds"
            :id="seconds"
            @change="validate"
          >
          seconds
        </label>
        <p v-show="error !== ''">{{error}}</p>
      </div>
      <div>
        <button @click="selectAll">Select All</button>
        <button @click="generator">Practice</button>
        <button @click=" autoGenerate()" :disabled="autoDisable">Auto</button>
        <button @click="stop">Stop</button>
        <button @click="clear">Clear</button>
      </div>

      <div>
        <p class="tag">{{key}} {{tonality}} {{technique}} {{area}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notes: [],
      techniques: [],
      tonalities: [],
      areas: [],
      key: "",
      tonality: "",
      technique: "",
      area: "",
      autoDisable: false,
      seconds: 60,
      error: ""
    };
  },
  methods: {
    generator() {
      this.key = this.notes[Math.floor(Math.random() * this.notes.length)];
      this.tonality = this.tonalities[
        Math.floor(Math.random() * this.tonalities.length)
      ];
      this.technique = this.techniques[
        Math.floor(Math.random() * this.techniques.length)
      ];
      this.area = this.areas[Math.floor(Math.random() * this.areas.length)];
    },

    autoGenerate() {
      this.generator();
      this.interval = setInterval(this.generator, this.seconds * 1000);
      this.autoDisable = true;
    },
    clear() {
      window.clearInterval(this.interval);
      window.clearInterval(this.changeInterval);
      this.key = "";
      this.tonality = "";
      this.technique = "";
      this.area = "";
      this.autoDisable = false;
      this.seconds = 60;
      this.notes = [];
      this.techniques = [];
      this.tonalities = [];
      this.areas = [];
    },
    stop() {
      window.clearInterval(this.interval);
      window.clearInterval(this.changeInterval);
      this.autoDisable = false;
    },
    validate() {
      this.seconds = Number(this.seconds);
      if (isNaN(this.seconds) === true) {
        this.error = "Please enter a valid number";
        this.seconds = 60;
      }
    },
    selectAll() {
      const checkBoxes = document.querySelectorAll('input[type="checkbox"]');
      checkBoxes.forEach(checkbox => {
        checkbox.checked = true;
      });
      this.notes = [
        "A  |  ",
        "A#/Bb  |  ",
        "B  |  ",
        "C  |  ",
        "C#/Db  |  ",
        "D  |  ",
        "D#/Eb  |  ",
        "E  |  ",
        "F  |  ",
        "F#/Gb  |  ",
        "G  |  ",
        "G#/Ab  |  "
      ];
      this.tonalities = ["Major  |  ", "Minor  |  "];
      this.techniques = ["Chords  |  ", "Scales  |  "];
      this.areas = [
        "Open",
        "Third Fret",
        "Fifth Fret",
        "Seventh Fret",
        "Tenth Fret",
        "Root E 6th String",
        "Root A",
        "Root D",
        "Root G",
        "Root B",
        "Root E 1st String"
      ];
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h2 {
  font-weight: 100;
}
label {
  font-size: 15px;
}
.options {
  display: flex;
  justify-content: space-evenly;
  margin: 0 10%;
}

.col {
  display: flex;
  flex-direction: column;
  border: 1px solid rgb(163, 163, 163);
  padding: 0px 3rem 20px 3rem;
  border-radius: 15px;
  background-color: rgba(255, 255, 255, 0.979);
  width: 15%;
  box-shadow: inset 0 0 40px rgb(139, 139, 139);
}

.bottom {
  background-color: white;
  padding: 2rem;
  margin: 3rem auto;
  width: 50%;
  box-shadow: inset 0 0 40px rgb(139, 139, 139);
  border-radius: 15px;
  background-color: rgba(255, 255, 255, 0.979);
}

.tag {
  font-size: 30px;
}

.bottom > label {
  font-size: 40px !important;
}

.col > h2 {
  margin: 20px 0 20px 0;
}

input[type="checkbox"] {
  width: 15px;
  height: 15px;
  background: white;
  border: 1px solid #555;
}

input[type="checkbox"]:checked {
  background: red;
}

button {
  padding: 10px 20px;
  margin: 20px 10px 0 10px;
  cursor: pointer;
  border-radius: 10px;
  outline: none;
}

input {
  font-size: 20px;
  padding: 1px 10px;
  width: 10%;
}

@media (max-width: 990px) {
  * {
    font-size: 10px;
  }
  h2 {
    font-size: 15px;
  }
  label {
    font-size: 10px;
  }

  .col {
    padding: 20px;
  }
}
@media (max-width: 700px) {
  .options {
    flex-direction: column;
    margin-left: 20%;
  }
  * {
    font-size: 20px;
  }
  h2 {
    font-size: 20px;
  }
  label {
    font-size: 20px;
  }

  .col {
    padding: 20px;
    margin: 20px;
    width: 60%;
  }
}
</style>
