<template>
  <div class="header">
    <a href="https://github.com/CodeBears/lunch-nanjing-fuxing" target="_blank">
      <i class="fa-brands fa-github fa-2xl"></i>
    </a>
  </div>
  <div class="container">
    <h1>吃午餐</h1>
    <div class="board">
      <button id="spin">抽</button>
      <div class="triangle" />
      <div class="spinner-table">
        <div class="dial">
          <div
            v-for="(choice, index) in choices"
            v-bind:key="index"
            class="slice"
          >
            <div class="label">
              {{ choice }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { CSSProperties, onMounted } from "vue";

const choices = [
  "柒",
  "粥",
  "好食肌",
  "海南雞",
  "超級食場",
  "烤肉飯",
  "12 mini",
  "subway",
  "江梅麵館",
  "速食",
];

if (choices.length % 2 != 0) {
  choices.push("踩雷");
}

const numberOfSlices = choices.length;
const circumference = (6.283185307 * 360) / 2;
const sliceHeight = circumference / numberOfSlices;
const sliceDegree = 360 / numberOfSlices;
let transformDegree = 0;

const setSliceStyle = (slices: HTMLDivElement[]) => {
  slices.forEach((slice) => {
    transformDegree += sliceDegree;

    slice.style.transform = `rotate(${transformDegree}deg)`;
    slice.style.height = `${sliceHeight}px`;
    slice.style.top = `calc(50% - ${sliceHeight / 2}px)`;
  });
};

const setLabelStyle = (labels: HTMLDivElement[]) => {
  labels.forEach((label) => {
    label.style.lineHeight = `${sliceHeight}px`;
  });
};

onMounted(() => {
  let addRule = (function (style) {
    let sheet = document.head.appendChild(style).sheet;
    return function (selector: string, css: Partial<CSSProperties>) {
      let propText = Object.entries(css)
        .map(function ([cssKey, cssValue]) {
          return cssKey + ":" + cssValue;
        })
        .join(";");
      sheet?.insertRule(selector + "{" + propText + "}", sheet.cssRules.length);
    };
  })(document.createElement("style"));

  const sliceBoardBottom = sliceHeight / 2 + 4;
  addRule(".slice:before", {
    "border-width": `0 0 ${sliceBoardBottom}px 175px`,
  });

  addRule(".slice:after", {
    "border-width": `0 175px ${sliceBoardBottom}px 0`,
  });

  const slices = [...document.querySelectorAll(".slice")] as HTMLDivElement[];
  const labels = [...document.querySelectorAll(".label")] as HTMLDivElement[];
  setSliceStyle(slices);
  setLabelStyle(labels);

  let rotation = 0;
  const spinButton = document.getElementById("spin") as HTMLButtonElement;
  spinButton.addEventListener("click", () => {
    rotation += Math.floor(720 + Math.random() * 359);

    const dial = document.querySelector(".dial") as HTMLDivElement;
    if (dial) {
      dial.style.transform = `rotate(${rotation}deg)`;
    }
  });
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$diameter: 350px;
$radius: ($diameter / 2);
$sliceColor: #d05a6e;

:root {
  --primary-color: #333;
}
.container {
  max-width: 800px;
  margin: 2rem auto;
  background-color: #eea9a9;
}

.spinner-table {
  width: ($diameter - 2px);
  height: ($diameter - 2px);
  position: relative;
  border-radius: 100%;
  overflow: hidden;
  -webkit-transform-style: preserve-3d; /* Opera, Chrome, and Safari */
}
.dial {
  height: 100%;
  transition: all 3s ease-out;
  letter-spacing: 2px;
  overflow: hidden;

  &:before {
    content: "";
    text-align: center;
    display: block;
    line-height: 60px;
    position: absolute;
    height: 40px;
    width: 40px;
    background: #eea9a9;
    box-shadow: 0 0 5px 5px rgba(#000, 0.1);
    top: 50%;
    left: 50%;
    margin-top: -20px;
    margin-left: -20px;
    border-radius: 100%;
    z-index: 200;
  }

  .slice {
    position: absolute;
    z-index: 150;
    left: 50%;
    width: 50%;
    color: white;
    text-align: right;
    padding-right: 10px;
    display: block;
    transform-origin: left center;
    overflow: hidden;

    &:before,
    &:after {
      content: "";
      display: block;
      width: 0;
      height: 0;
      border-style: solid;
    }

    &:before {
      margin-bottom: -1px;
      margin-top: -2px;
      border-color: transparent transparent $sliceColor transparent;
    }

    &:after {
      margin-top: -1px;
      margin-bottom: -2px;
      border-color: transparent $sliceColor transparent transparent;
    }

    &:nth-child(even) {
      &:after {
        border-color: transparent darken($sliceColor, 10%) transparent
          transparent;
      }
      &:before {
        border-color: transparent transparent darken($sliceColor, 10%)
          transparent;
      }
    }

    .label {
      position: absolute;
      top: 0;
      bottom: 0;
      width: 70%;
      padding-top: 1px;
      padding-bottom: 1px;
      font-size: 16px;
      text-align: right;
      padding-left: 20px;
    }
  }
}

.board {
  display: flex;
  align-items: center;
  flex-direction: column;
}

button {
  background: #d05a6e;
  border: 0;
  padding: 15px 50px;
  color: white;
  border-radius: 10px;
  margin: 30px;
  &:hover {
    cursor: pointer;
    background-color: #b5495b;
  }
}
.triangle {
  position: relative;
  top: 20px;
  z-index: 999;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 50px 17.5px 0 17.5px;
  border-color: #f8c3cd transparent transparent transparent;
}
.fa-github {
  color: #d05a6e;
  &:hover {
    cursor: pointer;
    color: #b5495b;
  }
}
.header {
  text-align: right;
}
</style>
