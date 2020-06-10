<template>
  <div id="app">
    <text-editor
      class="text-component"
      :style="topRowElement.builtInStyle"
      :id="topRowElement.id"
      :font-size="topRowElement.fontSize"
      :color="topRowElement.color"
      :background-color="topRowElement.backgroundColor"
      @choose:element="handleChooseElement"
    >
      {{ topRowElement.text }}
    </text-editor>
    <br />
    <text-editor
      v-for="e in bottomRowElements"
      :key="e.id"
      class="text-component"
      :style="e.builtInStyle"
      :id="e.id"
      :font-size="e.fontSize"
      :color="e.color"
      :background-color="e.backgroundColor"
      @choose:element="handleChooseElement"
      >
        {{ e.text }}
      </text-editor>
    <div class="controls">
      <textarea
        cols="40"
        rows="8"
        v-model="currentTextElement.text"
        class="text-field"
      ></textarea>
      <div class="controls__top">
        <div>
          <label for="bgcolor" class="controls__label">Background Color</label>
          <input
            type="color"
            id="bgcolor"
            name="bgcolor"
            v-model="currentTextElement.backgroundColor"
          />
        </div>

        <div>
          <label for="color" class="controls__label">Font Color</label>
          <input
            type="color"
            id="color"
            name="bgcolor"
            v-model="currentTextElement.color"
          />
        </div>

        <div>
          <label for="fontSize" class="controls__label">Font Size</label>
          <select id="fontSize" v-model="currentTextElement.fontSize">
            <option v-for="n in fontSizes" :value="n" :key="n">{{ n }}</option>
          </select>
        </div>
      </div>
      <div class="controls__bottom">
        <button @click="displayJson = !displayJson">Display JSON</button>
        <button @click="logJSON">Log JSON</button>
      </div>
      <div v-if="displayJson">
        <pre>{{ JSON.stringify(json, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import TextEditor from './components/TextEditor.vue';

export default Vue.extend({
  name: 'App',
  components: {
    TextEditor,
  },
  data: () => ({
    textElements: [
      {
        id: 0,
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px;',
        text: 'Hi',
        color: '#ffffff',
        backgroundColor: '#f8bbd0',
        fontSize: 32,
      },
      {
        id: 1,
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px',
        text: 'My lovely',
        color: '#ffffff',
        backgroundColor: '#f8bbd0',
        fontSize: 32,
      },
      {
        id: 2,
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px',
        text: 'little',
        color: '#880e4f',
        backgroundColor: '#fff',
        fontSize: 54,
      },
      {
        id: 3,
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px',
        text: 'Ponny',
        color: '#ba68c8',
        backgroundColor: '#f8bb00',
        fontSize: 54,
      },
    ],
    currentTextElementId: 0,
    displayJson: false,
  }),
  computed: {
    fontSizes() {
      const sizes = [];
      for (let i = 8; i <= 72; i += 1) sizes.push(i);
      return sizes;
    },
    currentTextElement() {
      return this.textElements.find((e) => e.id === this.currentTextElementId);
    },
    topRowElement() {
      return this.textElements[0];
    },
    bottomRowElements() {
      return this.textElements.slice(1);
    },
    json() {
      const { textElements } = this;
      const output = [textElements[0]];

      for (let i = 1; i < textElements.length; i += 1) {
        const prev = output[output.length - 1];
        const {
          text, color, backgroundColor, fontSize,
        } = textElements[i];
        if (
          prev.fontSize === fontSize
          && prev.color === color
          && prev.backgroundColor === backgroundColor
        ) {
          output[output.length - 1] = {
            text: `${prev.text} ${text}`,
            color: prev.color,
            backgroundColor: prev.backgroundColor,
            fontSize: prev.fontSize,
          };
        } else {
          output.push(text);
          output.push(color);
          output.push(backgroundColor);
          output.push(fontSize);
        }
      }

      return output;
    },
  },
  methods: {
    handleChooseElement(id) {
      this.currentTextElementId = id;
    },
    logJSON() {
      console.log(JSON.stringify(this.json, null, 2));
    },
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.text-component {
  cursor: pointer;
  border-radius: 6px;
  white-space: pre;
  font-family: Aleo;
  font-weight: 300;
  text-decoration: none;
  box-shadow: rgb(0, 0, 0) 0px 0px 0px 0px;
  text-transform: initial;
  font-style: normal;
  z-index: 2002;
  text-shadow: rgba(0, 0, 0, 0.2) 2px 2px 0px;
  line-height: 1.5em;
  margin-top: 60px;
  border: 0px solid rgb(70, 144, 247);
  padding: 10px;
  text-align: center;
  letter-spacing: 0px;
}

.controls {
  width: 80%;
  margin: 2rem auto;

  &__top {
    height: 100px;
    display: flex;
    flex-direction: column;
    margin: 1rem auto;
    justify-content: space-between;
  }

  &__bottom {
    width: 25%;
    margin: 1rem auto;
    display: flex;
    justify-content: space-between;
  }

  &__label {
    margin-right: 10px;
  }
}

.text-field {
  margin-top: 2rem;
  resize: none;
}

</style>
