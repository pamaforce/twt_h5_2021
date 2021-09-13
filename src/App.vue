<template>
  <div id="app">
    <div class="loading-class" v-show="page == -1">
      <div class="content-class">
        <img src="./assets/logo-twt.png" style="width: 125px; height: 20px" />
        <vm-progress :percentage="percent"></vm-progress>
      </div>
    </div>
    <div class="cover-class" v-show="page == 0">
      <div :class="['cube-left-class' + (statue ? ' leftcubemove' : '')]"></div>
      <div
        :class="['cube-right-class' + (statue ? ' rightcubemove' : '')]"
      ></div>
      <div class="text-class"></div>
      <!-- <div class="arrow-class"></div>
      <div class="logo-class"></div> -->
      <div class="logo-class"></div>
      <div class="start-class" @click="start"></div>
      <div :class="['arrow-class' + (statue ? ' arrowmove' : '')]"></div>
    </div>
    <div class="q1-class" v-show="page == 1">
      <div class="q1-1-class"></div>
      <div class="q1-2-class">
        <div class="q1-a-class" @click="pushChoice(1)"></div>
        <div class="q1-b-class" @click="pushChoice(2)"></div>
        <div class="q1-c-class" @click="pushChoice(3)"></div>
        <div class="q1-d-class" @click="pushChoice(4)"></div>
      </div>
    </div>
    <div class="q2-class" v-show="page == 2">
      <div class="q2-2-class">
        <div class="q2-a-class" @click="pushChoice(1)"></div>
        <div class="q2-b-class" @click="pushChoice(2)"></div>
        <div class="q2-c-class" @click="pushChoice(3)"></div>
        <div class="q2-d-class" @click="pushChoice(4)"></div>
      </div>
      <div class="q2-1-class"></div>
      <div class="back-class" @click="cancelChoice"></div>
    </div>
    <div class="q3-class" v-show="page == 3">
      <div class="q3-2-class">
        <div class="q3-a-class" @click="pushChoice(1)"></div>
        <div class="q3-b-class" @click="pushChoice(2)"></div>
        <div class="q3-c-class" @click="pushChoice(3)"></div>
        <div class="q3-d-class" @click="pushChoice(4)"></div>
      </div>
      <div class="q3-1-class"></div>
      <div class="back-class" @click="cancelChoice"></div>
    </div>
    <div class="q4-class" v-show="page == 4">
      <div class="q4-1-class"></div>
      <div class="q4-2-class">
        <div class="q4-a-class" @click="pushChoice(1)"></div>
        <div class="q4-b-class" @click="pushChoice(2)"></div>
        <div class="q4-c-class" @click="pushChoice(3)"></div>
        <div class="q4-d-class" @click="pushChoice(4)"></div>
      </div>
      <div class="back-class" @click="cancelChoice"></div>
    </div>
    <div class="q5-class" v-show="page == 5">
      <div class="q5-2-class"></div>
      <div class="q5-1-class"></div>
      <div class="q5-3-class"></div>
      <div class="q5-a-class" @click="pushChoice(1)"></div>
      <div class="q5-b-class" @click="pushChoice(2)"></div>
      <div class="q5-c-class" @click="pushChoice(3)"></div>
      <div class="q5-d-class" @click="pushChoice(4)"></div>
      <div class="back-class" @click="cancelChoice"></div>
    </div>
    <div class="q6-class" v-show="page == 6">
      <div class="q6-1-class"></div>
      <div class="q6-2-class">
        <div class="q6-a-class" @click="endChoice(1)"></div>
        <div class="q6-b-class" @click="endChoice(2)"></div>
        <div class="q6-c-class" @click="endChoice(3)"></div>
        <div class="q6-d-class" @click="endChoice(4)"></div>
      </div>
      <div class="q6-arrow-class"></div>
      <div class="back-class" @click="cancelChoice"></div>
    </div>
    <div class="result-class" v-show="page == 7">
      <div :class="'results-class ' + [results[ans]]">
        <div class="name-class">{{ userName }}</div>
        <div class="qrcode-class"></div>
        <div class="twt-class" @click="page++"></div>
      </div>
    </div>
    <div
      :class="'rec-class ' + [rec ? 'studio-class' : 'news-class']"
      v-show="page == 8"
    ></div>
  </div>
</template>

<script>
import { MessageBox } from "element-ui";
export default {
  name: "App",
  data: () => {
    return {
      showModal: false,
      statue: false,
      count: 0,
      percent: 0,
      page: -1,
      ans: 0,
      rec: 0,
      results: [
        "lake-class",
        "gym-class",
        "canteen-class",
        "library-class",
        "room-class",
      ],
      choice: [],
      userName: "",
    };
  },
  mounted: function () {
    this.preload();
  },
  watch: {
    page: function (val) {
      if (val == 0) {
        setTimeout(() => {
          this.statue = true;
        }, 100);
      } else {
        this.statue = false;
      }
    },
    count: function (val) {
      if (val === 35) {
        setTimeout(() => {
          this.page++;
        }, 100);
      }
    },
  },
  methods: {
    start() {
      MessageBox.prompt("", "请问您的昵称？^v^", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
      }).then(({ value }) => {
        this.userName = value;
        this.page++;
      });
    },
    pushChoice(index) {
      this.choice.push(index);
      this.page++;
    },
    cancelChoice() {
      this.choice.pop();
      this.page--;
    },
    endChoice(index) {
      this.choice.push(index);
      if (this.choice[1] == 1 || this.choice[1] == 3) this.ans = 3;
      else if (this.choice[2] != 2)
        this.ans = this.choice[2] == 1 ? 2 : this.choice[2];
      else if (this.choice[3] == 1 || this.choice[3] == 3) this.ans = 0;
      else if (this.choice[3] == 2 || this.choice[3] == 4)
        this.ans = this.choice[3] / 2;
      this.rec = this.choice[5] % 2;
      this.page++;
    },
    preload() {
      let imgs = [
        require("./assets/logo-twt.png"),
        require("./assets/img-arrow.png"),
        require("./assets/img-back.png"),
        require("./assets/img-cover.png"),
        require("./assets/img-leftcube.png"),
        require("./assets/img-q1-1.png"),
        require("./assets/img-q1-2.png"),
        require("./assets/img-q1-background.png"),
        require("./assets/img-q2-1.png"),
        require("./assets/img-q2-2.png"),
        require("./assets/img-q3-1.png"),
        require("./assets/img-q3-2.png"),
        require("./assets/img-q4-1.png"),
        require("./assets/img-q4-2.png"),
        require("./assets/img-q4-background.png"),
        require("./assets/img-q5-1.png"),
        require("./assets/img-q5-2.png"),
        require("./assets/img-q5-3.png"),
        require("./assets/img-q6-1.png"),
        require("./assets/img-q6-2.png"),
        require("./assets/img-q6-background.png"),
        require("./assets/img-rightcube.png"),
        require("./assets/img-canteen.png"),
        require("./assets/img-gym.png"),
        require("./assets/img-lake.png"),
        require("./assets/img-library.png"),
        require("./assets/img-news.png"),
        require("./assets/img-room.png"),
        require("./assets/img-studio.png"),
        require("./assets/arrow.png"),
        require("./assets/twt-logo.png"),
        require("./assets/start.png"),
        require("./assets/twt.png"),
        require("./assets/QRcode.png"),
        require("./assets/img-text.png"),
      ];
      for (let img of imgs) {
        let image = new Image();
        image.src = img;
        image.onload = () => {
          this.count++;
          this.percent = Math.floor((this.count / 35) * 100);
        };
      }
    },
  },
};
</script>

<style>
#app {
  position: relative;
  text-align: center;
  overflow: hidden;
}
.qrcode-class {
  position: absolute;
  left: 32px;
  bottom: 18px;
  width: 35px;
  height: 35px;
  border-radius: 4px;
  background-image: url("./assets/QRcode.png");
  background-size: 100% 100%;
}
.twt-class {
  position: absolute;
  right: 74px;
  bottom: 22px;
  width: 20px;
  height: 20px;
  background-image: url("./assets/twt.png");
  background-size: 100% 100%;
  animation: twtani 0.5s ease infinite;
  -webkit-animation: twtani 0.5s ease infinite;
}
.loading-class {
  height: 100vh;
  width: 100%;
}
.content-class {
  width: 300px;
  position: relative;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.cover-class {
  width: 100%;
  height: 100vh;
  background-image: url("./assets/img-cover.png");
  background-size: 100% 100%;
  background-position: center;
  background-repeat: no-repeat;
}
.start-class {
  width: 64.6vw;
  height: 20.8vw;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(calc(-50% + 9px), calc(-50% + 126px));
  background-image: url("./assets/start.png");
  background-size: 100% 100%;
  background-position: center;
  background-repeat: no-repeat;
  animation: startani 3s ease 1.5s infinite;
  -webkit-animation: startani 3s ease 1.5s infinite;
  transform-origin: center center;
  -moz-transform-origin: center center;
  -webkit-transform-origin: center center;
  -o-transform-origin: center center;
}
.back-class {
  background-image: url("./assets/img-back.png");
  background-size: 100% 100%;
  width: 30px;
  height: 30px;
  position: absolute;
  bottom: 35px;
  left: 50%;
  transform: translateX(calc(-50% - 155px));
}
.q1-class {
  width: 100%;
  height: 100vh;
  background-image: url("./assets/img-q1-background.png");
  background-size: 100% 100%;
}
.q2-class {
  width: 100%;
  height: 100vh;
  background-color: #9ab8ff;
}
.q3-class {
  width: 100%;
  height: 100vh;
  background-color: #ffffff;
}
.q4-class {
  width: 100%;
  height: 100vh;
  background-color: #373c66;
  background-image: url("./assets/img-q4-background.png");
  background-size: 100% 100%;
  background-position: center;
  background-repeat: no-repeat;
}
.q5-class {
  width: 100%;
  height: 100vh;
  background-color: #f2f2f2;
}
.q6-class {
  width: 100%;
  height: 100vh;
  background-color: #c7c1fd;
  background-image: url("./assets/img-q6-background.png");
  background-size: 100% 100%;
  background-position: center;
  background-repeat: no-repeat;
}
.result-class {
  width: 100%;
  height: 100vh;
}
.results-class {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  height: 100vh;
  background-size: 100% 100%;
}
.rec-class {
  width: 100%;
  height: 100vh;
  background-color: #ffffff;
  background-size: 100% 100%;
  background-position: center;
  background-repeat: no-repeat;
}
.news-class {
  background-image: url("./assets/img-news.png");
}
.studio-class {
  background-image: url("./assets/img-studio.png");
}
.name-class {
  text-align: center;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  width: 90px;
  color: #1b1b1c;
  font-family: Hyk1gj;
  font-size: 20px;
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(calc(-50% - 143px));
}
.lake-class {
  background-image: url("./assets/img-lake.png");
}
.gym-class {
  background-image: url("./assets/img-gym.png");
}
.canteen-class {
  background-image: url("./assets/img-canteen.png");
}
.room-class {
  background-image: url("./assets/img-room.png");
}
.library-class {
  background-image: url("./assets/img-library.png");
}
.cube-left-class {
  position: absolute;
  left: -120px;
  top: -225px;
  width: 115px;
  height: 225px;
  background-image: url("./assets/img-leftcube.png");
  background-size: 100% 100%;
  transition: all 3s ease;
}
.cube-right-class {
  position: absolute;
  right: -320px;
  top: -500px;
  width: 320px;
  height: 500px;
  background-image: url("./assets/img-rightcube.png");
  background-size: 100% 100%;
  transition: all 3s ease;
}
.logo-class {
  background-image: url("./assets/twt-logo.png");
  background-size: 100% 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(calc(-50% + 100px), calc(-50% + 50px));
  width: 80px;
  height: 80px;
}
.text-class {
  background-image: url("./assets/img-text.png");
  background-size: 100% 100%;
  width: 306px;
  height: 193px;
  position: absolute;
  left: -306px;
  top: 50%;
  transform: translateY(calc(-50% - 50px));
  animation: textin 1.5s ease 1;
  -webkit-animation: textin 1.5s ease 1;
  animation-fill-mode: forwards;
}
.arrow-class {
  background-image: url("./assets/arrow.png");
  background-size: 100% 100%;
  position: absolute;
  right: -80px;
  top: 700px;
  width: 45px;
  height: 42px;
  transition: all 3s ease;
}
.q1-1-class {
  background-image: url("./assets/img-q1-1.png");
  background-size: 100% 100%;
  width: 320px;
  height: 36px;
  position: absolute;
  right: -320px;
  top: 72px;
  animation: q1labelin 0.5s ease 1;
  -webkit-animation: q1labelin 0.5s ease 1;
  animation-fill-mode: forwards;
}
.q2-1-class {
  background-image: url("./assets/img-q2-1.png");
  background-size: 100% 100%;
  width: 100%;
  height: 36px;
  position: absolute;
  right: -320px;
  top: 72px;
  animation: q2labelin 0.5s ease 1;
  -webkit-animation: q2labelin 0.5s ease 1;
  animation-fill-mode: forwards;
}
.q3-1-class {
  background-image: url("./assets/img-q3-1.png");
  background-size: 100% 100%;
  width: 100%;
  height: 36px;
  position: absolute;
  left: -340px;
  top: 72px;
  animation: q3labelin 0.5s ease 1;
  -webkit-animation: q3labelin 0.5s ease 1;
  animation-fill-mode: forwards;
}
.q4-1-class {
  background-image: url("./assets/img-q4-1.png");
  background-size: 100% 100%;
  width: 0;
  height: 36px;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 72px;
  animation: q4labelin 0.5s ease 1;
  -webkit-animation: q4labelin 0.5s ease 1;
  animation-fill-mode: forwards;
}
.q5-1-class {
  background-image: url("./assets/img-q5-1.png");
  background-size: 100% 100%;
  width: 0;
  height: 240px;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 340px;
  animation: q5labelin 0.5s ease 1;
  -webkit-animation: q5labelin 0.5s ease 1;
  animation-fill-mode: forwards;
}
.q6-1-class {
  background-image: url("./assets/img-q6-1.png");
  background-size: 100% 100%;
  width: 365px;
  height: 36px;
  position: absolute;
  right: -320px;
  top: 72px;
  animation: q6labelin 0.5s ease 1;
  -webkit-animation: q6labelin 0.5s ease 1;
  animation-fill-mode: forwards;
}
.q1-2-class {
  background-image: url("./assets/img-q1-2.png");
  background-size: 100% 100%;
  width: 300px;
  height: 540px;
  position: absolute;
  left: 50%;
  top: 40px;
  transform: translateX(-50%);
  opacity: 0.3;
  animation: allin 2s ease 1;
  -webkit-animation: allin 2s ease 1;
  animation-fill-mode: forwards;
}
.q2-2-class {
  background-image: url("./assets/img-q2-2.png");
  background-size: 100% 100%;
  width: 100%;
  height: 100vh;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 0;
  opacity: 0.3;
  animation: allin 2s ease 1;
  -webkit-animation: allin 2s ease 1;
  animation-fill-mode: forwards;
}
.q3-2-class {
  background-image: url("./assets/img-q3-2.png");
  background-size: 100% 100%;
  width: 100%;
  height: 100vh;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 0;
  opacity: 0.3;
  animation: allin 2s ease 1;
  -webkit-animation: allin 2s ease 1;
  animation-fill-mode: forwards;
}
.q4-2-class {
  background-image: url("./assets/img-q4-2.png");
  background-size: 100% 100%;
  width: 335px;
  height: 580px;
  position: absolute;
  left: 50%;
  transform: translateX(calc(-50% + 10px));
  top: 42px;
  opacity: 0.3;
  animation: allin 2s ease 1;
  -webkit-animation: allin 2s ease 1;
  animation-fill-mode: forwards;
}
.q5-2-class {
  background-image: url("./assets/img-q5-2.png");
  background-size: 100% 100%;
  width: 100%;
  height: 100vh;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 0;
  opacity: 0.3;
  animation: allin 2s ease 1;
  -webkit-animation: allin 2s ease 1;
  animation-fill-mode: forwards;
}
.q6-2-class {
  background-image: url("./assets/img-q6-2.png");
  background-size: 100% 100%;
  width: 330px;
  height: 475px;
  position: absolute;
  left: 50%;
  transform: translateX(calc(-50%));
  top: 40px;
  opacity: 0.3;
  animation: allin 2s ease 1;
  -webkit-animation: allin 2s ease 1;
  animation-fill-mode: forwards;
}
.q5-3-class {
  background-image: url("./assets/img-q5-3.png");
  background-size: 100% 100%;
  width: 215px;
  height: 180px;
  position: absolute;
  left: 50%;
  transform: translateX(calc(-50% - 40px));
  top: 85px;
  animation: q5textmove 1s ease 1;
  -webkit-animation: q5textmove 1s ease 1;
  animation-fill-mode: forwards;
}
.q6-arrow-class {
  background-image: url("./assets/img-arrow.png");
  background-size: 100% 100%;
  width: 50px;
  height: 50px;
  position: absolute;
  right: -50px;
  top: 365px;
  animation: q6arrowmove 1s ease 1;
  -webkit-animation: q6arrowmove 1s ease 1;
  animation-fill-mode: forwards;
}
.q1-a-class {
  position: absolute;
  width: 220px;
  height: 80px;
  top: 145px;
  left: 60px;
}
.q1-b-class {
  position: absolute;
  width: 220px;
  height: 80px;
  top: 235px;
  left: 60px;
}
.q1-c-class {
  position: absolute;
  width: 220px;
  height: 80px;
  top: 325px;
  left: 60px;
}
.q1-d-class {
  position: absolute;
  width: 220px;
  height: 80px;
  top: 415px;
  left: 60px;
}
.q2-a-class {
  position: absolute;
  width: 275px;
  height: 130px;
  top: 125px;
  left: 55px;
}
.q2-b-class {
  position: absolute;
  width: 265px;
  height: 120px;
  top: 255px;
  left: 85px;
}
.q2-c-class {
  position: absolute;
  width: 270px;
  height: 115px;
  top: 375px;
  left: 15px;
}
.q2-d-class {
  position: absolute;
  width: 265px;
  height: 100px;
  top: 490px;
  left: 70px;
}
.q3-a-class {
  position: absolute;
  width: 330px;
  height: 81px;
  top: 200px;
  left: 0;
}
.q3-b-class {
  position: absolute;
  width: 330px;
  height: 81px;
  top: 281px;
  left: 0;
}
.q3-c-class {
  position: absolute;
  width: 375px;
  height: 81px;
  top: 363px;
  left: 0;
}
.q3-d-class {
  position: absolute;
  width: 330px;
  height: 83px;
  top: 443px;
  left: 0;
}
.q4-a-class {
  position: absolute;
  width: 270px;
  height: 75px;
  top: 123px;
  left: 15px;
}
.q4-b-class {
  position: absolute;
  width: 252px;
  height: 90px;
  top: 208px;
  left: 70px;
}
.q4-c-class {
  position: absolute;
  width: 245px;
  height: 85px;
  top: 315px;
  left: 0px;
}
.q4-d-class {
  position: absolute;
  width: 295px;
  height: 80px;
  top: 420px;
  left: 45px;
}
.q5-a-class {
  position: absolute;
  width: 345px;
  height: 62px;
  top: 330px;
  left: 50%;
  transform: translateX(-50%);
}
.q5-b-class {
  position: absolute;
  width: 345px;
  height: 65px;
  top: 392px;
  left: 50%;
  transform: translateX(-50%);
}
.q5-c-class {
  position: absolute;
  width: 345px;
  height: 65px;
  top: 457px;
  left: 50%;
  transform: translateX(-50%);
}
.q5-d-class {
  position: absolute;
  width: 345px;
  height: 65px;
  top: 522px;
  left: 50%;
  transform: translateX(-50%);
}
.q6-a-class {
  position: absolute;
  width: 170px;
  height: 90px;
  top: 120px;
  left: 15px;
}
.q6-b-class {
  position: absolute;
  width: 210px;
  height: 65px;
  top: 230px;
  left: 130px;
}
.q6-c-class {
  position: absolute;
  width: 210px;
  height: 65px;
  top: 325px;
  left: 25px;
}
.q6-d-class {
  position: absolute;
  width: 210px;
  height: 65px;
  top: 420px;
  left: 82px;
}
.el-message-box {
  width: 250px;
}
.el-message-box__input {
  padding-top: 5px;
  height: 50px;
}
.el-input__inner {
  text-align: center;
}
.el-message-box__content {
  padding: 0 15px;
}
.leftcubemove {
  left: 50%;
  top: 50%;
  transform: translate(calc(-50% - 120px), calc(-50% - 190px));
  animation: leftcube 3s ease infinite;
  -webkit-animation: leftcube 3s ease infinite;
}
.rightcubemove {
  right: 50%;
  top: 50%;
  transform: translate(calc(50% + 20px), calc(-50% - 40px));
  animation: rightcube 3s ease 4s infinite;
  -webkit-animation: rightcube 3s ease 4s infinite;
}
.arrowmove {
  right: 50%;
  top: 50%;
  transform: translate(calc(50% + 100px), calc(-50% + 135px));
  animation: arrow 3s ease 3s infinite;
  -webkit-animation: arrow 3s ease 3s infinite;
}
@keyframes twtani {
  0% {
    transform: rotate(0);
  }
  50% {
    transform: rotate(10deg);
  }
  100% {
    transform: rotate(0);
  }
}
@keyframes textin {
  0% {
    left: -306px;
  }
  70% {
    left: 50%;
    transform: translateX(calc(-50% + 50px)) translateY(calc(-50% - 50px));
  }
  100% {
    left: 50%;
    transform: translateX(-50%) translateY(calc(-50% - 50px));
  }
}
@keyframes startani {
  0% {
    transform: scale(1) translate(calc(-50% + 9px), calc(-50% + 126px));
  }
  50% {
    transform: scale(1.05) translate(calc(-50% + 9px), calc(-50% + 124px));
  }
  100% {
    transform: scale(1) translate(calc(-50% + 9px), calc(-50% + 126px));
  }
}
@keyframes leftcube {
  0% {
    top: 50%;
  }
  50% {
    top: calc(50% + 40px);
  }
  100% {
    top: 50%;
  }
}
@keyframes rightcube {
  0% {
    top: 50%;
  }
  50% {
    top: calc(50% + 30px);
  }
  100% {
    top: 50%;
  }
}
@keyframes arrow {
  0% {
    top: 50%;
    right: 50%;
  }
  50% {
    top: calc(50% + 10px);
    right: calc(50% - 20px);
  }
  100% {
    top: 50%;
    right: 50%;
  }
}
@keyframes arrowmove {
  0% {
    right: -80px;
    bottom: -80px;
  }
  100% {
    right: 50%;
    bottom: 50%;
    transform: translate(calc(50%), calc(50% + 150px));
  }
}
@keyframes allout {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
@keyframes allin {
  0% {
    opacity: 0.3;
  }
  100% {
    opacity: 1;
  }
}
@keyframes q1labelin {
  0% {
    right: -340px;
  }
  100% {
    right: 50%;
    transform: translateX(calc(50% + 40px));
  }
}
@keyframes q2labelin {
  0% {
    right: -340px;
  }
  100% {
    right: 50%;
    transform: translateX(50%);
  }
}
@keyframes q3labelin {
  0% {
    left: -340px;
  }
  100% {
    left: 50%;
    transform: translateX(-50%);
  }
}
@keyframes q4labelin {
  0% {
    width: 0;
  }
  100% {
    width: 352px;
  }
}
@keyframes q5labelin {
  0% {
    width: 0;
  }
  100% {
    width: 320px;
  }
}
@keyframes q6labelin {
  0% {
    right: -340px;
  }
  100% {
    right: 50%;
    transform: translateX(calc(50% + 15px));
  }
}
@keyframes q5textmove {
  0% {
    transform: translateX(calc(-50% - 40px));
    top: 85px;
  }
  100% {
    transform: translateX(calc(-50% - 30px));
    top: 70px;
  }
}
@keyframes q6arrowmove {
  0% {
    right: -50px;
    top: 365px;
  }
  100% {
    right: 50%;
    transform: translateX(calc(50% + 130px));
    top: 345px;
  }
}

@media screen and (min-width: 380px) {
  .name-class {
    transform: translateX(calc(-50% - 150px));
  }
}
@media screen and (min-width: 400px) {
  .name-class {
    transform: translateX(calc(-50% - 158px));
  }
}
@media screen and (max-width: 360px) {
  .name-class {
    transform: translateX(calc(-50% - 136px));
  }
}
@media screen and (max-width: 340px) {
  .name-class {
    font-size: 19px;
    transform: translateX(calc(-50% - 128px));
  }
}
@media screen and (max-width: 330px) {
  .name-class {
    font-size: 19px;
    transform: translateX(calc(-50% - 123px));
  }
}
@media screen and (max-width: 300px) {
  .name-class {
    transform: translateX(calc(-50% - 143px));
  }
  .q4-class,
  .cover-class,
  .q6-class,
  .rec-class {
    background-size: 375px 100%;
  }
  .results-class,
  .q2-1-class,
  .q2-2-class,
  .q3-1-class,
  .q3-2-class,
  .q5-2-class {
    width: 375px;
  }
  .start-class {
    width: 230px;
    height: 68px;
  }
}
@media screen and (min-width: 450px) {
  .name-class {
    transform: translateX(calc(-50% - 143px));
  }
  .q4-class,
  .cover-class,
  .q6-class,
  .rec-class {
    background-size: 375px 100%;
  }
  .results-class,
  .q2-1-class,
  .q2-2-class,
  .q3-1-class,
  .q3-2-class,
  .q5-2-class {
    width: 375px;
  }
  .start-class {
    width: 230px;
    height: 68px;
  }
}
@media screen and (max-height: 640px) {
  .q1-class,
  .q2-class,
  .q3-class,
  .q4-class,
  .cover-class,
  .q5-class,
  .q6-class,
  .loading-class,
  .cover-class,
  .results-class,
  .q2-2-class,
  .q3-2-class,
  .q5-2-class,
  .result-class {
    height: 667px;
  }
}
@media screen and (min-height: 700px) {
  .q1-class,
  .q2-class,
  .q3-class,
  .q4-class,
  .cover-class,
  .q5-class,
  .q6-class,
  .loading-class,
  .cover-class,
  .results-class,
  .q2-2-class,
  .q3-2-class,
  .q5-2-class,
  .result-class {
    height: 667px;
  }
}
</style>
