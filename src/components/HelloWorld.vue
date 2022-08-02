<template>
  <div
    class="conteiner"
    ref="box"
    :style="{
      fontSize: parseInt(font) > 5 ? font + 'px' : false,
      height: persentTo,
    }"
  >
    <ul class="hashtag-box">
      <div class="facke hidden" ref="facke"></div>

      <li class="hashtag" v-for="(hash, index) in handlerShow" :key="index">
        <div class="hashtag-separator">
          <svg
            width="100%"
            height="100%"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 490.1 490.1"
          >
            <g>
              <path
                d="M234.1,29.05l-44.3,136.1c-1.6,4.7-5.8,7.8-10.5,7.8H0l145.1,105.4c3.9,2.7,5.4,7.8,3.9,12.4l-55.3,170.3l140.4-101.9 L234.1,29.05z"
              />
              <path
                d="M300.2,165.15l-44.3-136.1v329.8l140.4,101.9l-55.2-170.3c-1.6-4.7,0-9.7,3.9-12.4l145.1-105H310.7 C306.1,172.95,301.8,169.85,300.2,165.15z"
              />
            </g>
          </svg>
        </div>
        <div class="hashtag-tag">
          {{ hash }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Hashtag",
  props: {
    hashtag: {
      type: Array,
      required: true,
    },
    font: String,
    lineHeght: String,
  },
  data() {
    return {
      handlerShow: [],
      separator: `<div class="hashtag-separator"><svg width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 490.1 490.1"><g><path d="M234.1,29.05l-44.3,136.1c-1.6,4.7-5.8,7.8-10.5,7.8H0l145.1,105.4c3.9,2.7,5.4,7.8,3.9,12.4l-55.3,170.3l140.4-101.9 L234.1,29.05z"/> <path d="M300.2,165.15l-44.3-136.1v329.8l140.4,101.9l-55.2-170.3c-1.6-4.7,0-9.7,3.9-12.4l145.1-105H310.7 C306.1,172.95,301.8,169.85,300.2,165.15z"/></g></svg></div>`,
    };
  },
  computed: {
    persentTo() {
      const pixel = (window.innerHeight / 100) * parseInt(this.lineHeght);
      return parseInt(this.lineHeght) > 0 ? pixel + "px" : "auto";
    },
  },
  methods: {
    resize(e) {
      this.handler(this.hashtag);
    },
    handler(arr) {
      if (arr.length) {
        let offsetItems = 0,
          showItem = true;

        const boxOffset = this.$refs.box.offsetWidth,
          showItems = arr.filter((i) => {
            const fackeBox = this.$refs.facke,
              fackeBoxOffset = fackeBox.offsetWidth;

            fackeBox.innerHTML = document.createElement("div").innerHTML =
              this.separator +
              '<div class="hashtag-tag">' +
              i.trim() +
              "</div>";

            if (
              offsetItems + fackeBoxOffset < boxOffset - fackeBoxOffset &&
              showItem
            ) {
              offsetItems += fackeBoxOffset;
              return i.trim();
            } else if (
              offsetItems + fackeBoxOffset >=
              boxOffset - fackeBoxOffset
            ) {
              showItem = false;
            }
          });
        this.handlerShow = showItems;
      }
    },
  },
  watch: {
    hashtag(newVal) {
      this.handler(newVal);
    },
    font() {
      this.handler(this.hashtag);
    },
    height() {
      this.handler(this.hashtag);
    },
    deep: true,
  },
  created() {
    window.addEventListener("resize", this.resize);
  },
  destroyed() {
    window.removeEventListener("resize", this.resize);
  },
};
</script>

<style lang="scss">
.conteiner {
  max-width: calc(100% - 10px);
  width: 100%;
  font-size: 1em;
  display: flex;

  & > .hashtag-box {
    list-style: none;
    display: block;
    width: max-content;

    .hashtag,
    .hidden {
      float: left;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-flow: row wrap;

      .hashtag-separator {
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0 10px;
        width: 1em;
      }
      .hashtag-tag {
        font-weight: normal;
      }
    }
  }
  .hidden {
    position: absolute;
    top: -100px;
    opacity: 0;
    z-index: -999;
    height: 0;
    width: fit-content;
  }
}
</style>
