<template>
  <div id="app">
    <div class="loading-class" v-show="page == -1">
      <div class="content-class">
        <img src="./assets/logo-twt.png" style="width: 125px; height: 20px" />
        <vm-progress :percentage="percent"></vm-progress>
      </div>
    </div>
    <div class="cover-class" v-show="page == 0">
      <div class="cube-left-class"></div>
      <div class="cube-right-class"></div>
      <div class="center-class"></div>
      <div class="arrow-class"></div>
      <div class="logo-class"></div>
      <div class="next-class" @click="page++"></div>
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
      <div class="q2-1-class"></div>
      <div class="q2-2-class">
        <div class="q2-a-class" @click="pushChoice(1)"></div>
        <div class="q2-b-class" @click="pushChoice(2)"></div>
        <div class="q2-c-class" @click="pushChoice(3)"></div>
        <div class="q2-d-class" @click="pushChoice(4)"></div>
      </div>
      <div class="back-class" @click="cancelChoice"></div>
    </div>
    <div class="q3-class" v-show="page == 3">
      <div class="q3-1-class"></div>
      <div class="q3-2-class">
        <div class="q3-a-class" @click="pushChoice(1)"></div>
        <div class="q3-b-class" @click="pushChoice(2)"></div>
        <div class="q3-c-class" @click="pushChoice(3)"></div>
        <div class="q3-d-class" @click="pushChoice(4)"></div>
      </div>
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
      <div :class="'results-class ' + [results[ans]]" id="capture">
        <div class="name-class">{{ userName }}</div>
        <div class="button-class" @click="turnToImg" v-show="clicked"></div>
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
import html2canvas from "html2canvas";
export default {
  name: "App",
  data: () => {
    return {
      showModal: false,
      clicked: true,
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
    count: function (val) {
      if (val === 33) {
        setTimeout(() => {
          this.page++;
        }, 100);
      }
    },
  },
  methods: {
    turnToImg() {
      this.clicked = false;
      setTimeout(() => {
        html2canvas(document.querySelector("#capture"), { scale: 2 }).then(
          (canvas) => {
            var image = new Image();
            image.src = canvas.toDataURL("image/png");
            console.log(image);
            this.page++;
          }
        );
      }, 0);
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
      MessageBox.prompt("", "请问您的昵称？", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
      }).then(({ value }) => {
        this.userName = value;
        if (this.choice[1] == 1 || this.choice[1] == 3) this.ans = 3;
        else if (this.choice[2] != 1) this.ans = this.choice[2];
        else if (this.choice[3] == 1 || this.choice[3] == 3) this.ans = 0;
        else if (this.choice[3] == 2 || this.choice[3] == 4)
          this.ans = this.choice[3] / 2;
        this.rec = this.choice[5] % 2;
        this.page++;
      });
    },
    preload() {
      let imgs = [
        require("./assets/logo-twt.png"),
        require("./assets/img-arrow.png"),
        require("./assets/img-back.png"),
        require("./assets/img-center.png"),
        require("./assets/img-cover.png"),
        require("./assets/img-leftcube.png"),
        require("./assets/img-q1-1.png"),
        require("./assets/img-q1-2.png"),
        require("./assets/img-q1-background.png"),
        require("./assets/img-q2-1.png"),
        require("./assets/img-q2-2.png"),
        require("./assets/img-q2-background.png"),
        require("./assets/img-q3-1.png"),
        require("./assets/img-q3-2.png"),
        require("./assets/img-q3-background.png"),
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
        require("./assets/img-button.png"),
        require("./assets/img-canteen.png"),
        require("./assets/img-gym.png"),
        require("./assets/img-lake.png"),
        require("./assets/img-library.png"),
        require("./assets/img-news.png"),
        require("./assets/img-room.png"),
        require("./assets/img-studio.png"),
      ];
      for (let img of imgs) {
        let image = new Image();
        image.src = img;
        image.onload = () => {
          this.count++;
          this.percent = Math.floor((this.count / 33) * 100);
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
  background-size: 130% 130%;
  background-repeat: no-repeat;
  animation: covermove 5s ease-in-out infinite;
  -webkit-animation: covermove 5s ease-in-out infinite;
  background-position: 30% 30%;
}
.back-class {
  background-image: url("./assets/img-back.png");
  background-size: 100% 100%;
  width: 18px;
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
  background-image: url("./assets/img-q2-background.png");
  background-size: 100% 100%;
}
.q3-class {
  width: 100%;
  height: 100vh;
  background-image: url("./assets/img-q3-background.png");
  background-size: 100% 100%;
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
  background-color: #c7c1fd;
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
  background-color: #ffa5aa;
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
  width: 110px;
  height: 26px;
  color: #1b1b1c;
  font-family: Hyk1gj;
  font-size: 26px;
  position: absolute;
  top: 11.7%;
  left: 50%;
  transform: translateX(calc(-50% - 128px));
}
.button-class {
  position: absolute;
  left: 50%;
  transform: translateX(calc(-50% + 5px));
  bottom: 15px;
  background-image: url("./assets/img-button.png");
  background-size: 100% 100%;
  width: 320px;
  height: 80px;
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
  animation: leftcubemove 3s ease 1;
  -webkit-animation: leftcubemove 3s ease 1;
  animation-fill-mode: forwards;
}
.cube-right-class {
  position: absolute;
  right: -320px;
  top: -500px;
  width: 320px;
  height: 500px;
  background-image: url("./assets/img-rightcube.png");
  background-size: 100% 100%;
  animation: rightcubemove 3s ease 1;
  -webkit-animation: rightcubemove 3s ease 1;
  animation-fill-mode: forwards;
}
.logo-class {
  background-image: url("./assets/logo-twt.png");
  background-size: 100% 100%;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: 20px;
  width: 125px;
  height: 20px;
}
.center-class {
  background-image: url("./assets/img-center.png");
  background-size: 100% 100%;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 0px;
  width: 100%;
  height: 625px;
  animation: allin 2s ease 1;
  -webkit-animation: allin 2s ease 1;
  animation-fill-mode: forwards;
  opacity: 0.3;
}
.arrow-class {
  background-image: url("./assets/img-arrow.png");
  background-size: 100% 100%;
  position: absolute;
  right: -80px;
  bottom: -80px;
  width: 75px;
  height: 75px;
  animation: arrowmove 2s ease 1;
  -webkit-animation: arrowmove 2s ease 1;
  animation-fill-mode: forwards;
}
.next-class {
  width: 180px;
  height: 180px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(calc(-50% - 40px), calc(-50% + 105px));
  border-radius: 50% 50%;
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
  height: 520px;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 40px;
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
  width: 255px;
  height: 115px;
  top: 140px;
  left: 45px;
}
.q2-b-class {
  position: absolute;
  width: 260px;
  height: 120px;
  top: 255px;
  left: 85px;
}
.q2-c-class {
  position: absolute;
  width: 265px;
  height: 115px;
  top: 375px;
  left: 10px;
}
.q2-d-class {
  position: absolute;
  width: 235px;
  height: 110px;
  top: 490px;
  left: 70px;
}
.q3-a-class {
  position: absolute;
  width: 260px;
  height: 73px;
  top: 255px;
  left: 40px;
}
.q3-b-class {
  position: absolute;
  width: 192px;
  height: 62px;
  top: 328px;
  left: 38px;
}
.q3-c-class {
  position: absolute;
  width: 375px;
  height: 68px;
  top: 390px;
  left: 0px;
}
.q3-d-class {
  position: absolute;
  width: 242px;
  height: 70px;
  top: 458px;
  left: 33px;
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
@keyframes covermove {
  0% {
    background-position: 30% 30%;
  }
  25% {
    background-position: 0% 30%;
  }
  50% {
    background-position: 0% 0%;
  }
  75% {
    background-position: 30% 0%;
  }
  100% {
    background-position: 30% 30%;
  }
}
@keyframes leftcubemove {
  0% {
    left: -120px;
    top: -225px;
  }
  100% {
    left: 50%;
    top: 50%;
    transform: translate(calc(-50% - 90px), calc(-50% - 200px));
  }
}
@keyframes rightcubemove {
  0% {
    right: -320px;
    top: -500px;
  }
  100% {
    right: 50%;
    top: 50%;
    transform: translate(calc(50% + 20px), calc(-50% - 40px));
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
    height: 25px;
    font-size: 25px;
    top: 11.7%;
    transform: translateX(calc(-50% - 128px));
  }
}
@media screen and (min-width: 400px) {
  .name-class {
    height: 25px;
    font-size: 25px;
    top: 11.7%;
    transform: translateX(calc(-50% - 138px));
  }
}
@media screen and (max-width: 360px) {
  .name-class {
    height: 24px;
    font-size: 24px;
    top: 11.7%;
    transform: translateX(calc(-50% - 120px));
  }
}
@media screen and (max-width: 340px) {
  .name-class {
    height: 24px;
    font-size: 24px;
    top: 11.7%;
    transform: translateX(calc(-50% - 115px));
  }
}
@media screen and (max-width: 330px) {
  .name-class {
    height: 24px;
    font-size: 24px;
    top: 11.7%;
    transform: translateX(calc(-50% - 112px));
  }
}
@media screen and (max-width: 300px) {
  .center-class {
    width: 375px;
  }
  .q4-class {
    background-size: 375px 100%;
  }
  .q6-class {
    background-size: 375px 100%;
  }
  .rec-class {
    background-size: 375px 100%;
  }
  .results-class {
    width: 375px;
  }
  .q2-1-class {
    width: 375px;
  }
  .q2-2-class {
    width: 375px;
  }
  .q3-1-class {
    width: 375px;
  }
  .q3-2-class {
    width: 375px;
  }
  .q5-2-class {
    width: 375px;
  }
}
@media screen and (min-width: 450px) {
  .center-class {
    width: 375px;
  }
  .q4-class {
    background-size: 375px 100%;
  }
  .q6-class {
    background-size: 375px 100%;
  }
  .rec-class {
    background-size: 375px 100%;
  }
  .results-class {
    width: 375px;
  }
  .q2-1-class {
    width: 375px;
  }
  .q2-2-class {
    width: 375px;
  }
  .q3-1-class {
    width: 375px;
  }
  .q3-2-class {
    width: 375px;
  }
  .q5-2-class {
    width: 375px;
  }
}
@media screen and (max-height: 640px) {
  .q1-class {
    height: 667px;
  }
  .q2-class {
    height: 667px;
  }
  .q3-class {
    height: 667px;
  }
  .q4-class {
    height: 667px;
  }
  .q5-class {
    height: 667px;
  }
  .q6-class {
    height: 667px;
  }
  .loading-class {
    height: 667px;
  }
  .cover-class {
    height: 667px;
  }
  .results-class {
    height: 667px;
  }
  .q2-2-class {
    height: 667px;
  }
  .q5-2-class {
    height: 667px;
  }
}
@media screen and (min-height: 700px) {
  .q1-class {
    height: 667px;
  }
  .q2-class {
    height: 667px;
  }
  .q3-class {
    height: 667px;
  }
  .q4-class {
    height: 667px;
  }
  .q5-class {
    height: 667px;
  }
  .q6-class {
    height: 667px;
  }
  .loading-class {
    height: 667px;
  }
  .cover-class {
    height: 667px;
  }
  .results-class {
    height: 667px;
  }
  .q2-2-class {
    height: 667px;
  }
  .q5-2-class {
    height: 667px;
  }
}
</style>
