<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />

    <h2>Test Hashtag</h2>

    <label>
      <h3>enter font size:</h3>
      <input
        class="enter-font"
        type="number"
        v-model="fonts"
        step="1"
        min="6"
        max="120"
      />px
    </label>

    <label>
      <h3>enter the block height:</h3>
      <input
        class="enter-height"
        type="number"
        v-model="heights"
        step="3"
        min="0"
        max="100"
      />%
    </label>

    <label>
      <h3>enter the hashtag separated by commas:</h3>
      <input
        class="enter-tags"
        type="text"
        v-model="input"
        placeholder="example: video, move, show"
      />
    </label>

    <HelloWorld :hashtag="hashtag" :font="fonts" :lineHeght="heights" />
    <img alt="Vue logo" src="./assets/logo.png" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      input: "",
      fonts: "0",
      heights: "0",
    };
  },
  computed: {
    hashtag() {
      return this.input.split(",");
    },
  },
};
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

  & > label .enter-tags {
    width: calc(100% - 30px);
    min-height: 40px;
    border: none;
    border-radius: 5px;
    box-shadow: 0 0 5px skyblue;
    padding: 4px 10px;
    margin: 0 0 40px;
  }
  & > label .enter-font,
  & > label .enter-height {
    min-height: 20px;
    border: none;
    border-radius: 5px;
    box-shadow: 0 0 5px skyblue;
    padding: 4px 10px;
  }
}
</style>
