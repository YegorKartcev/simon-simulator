<template>
  <div class="container">
    <div class="ribbon"><span><a class="ribbon-link" href="https://github.com/YegorKartcev">GitHub</a></span></div>
    <div class="m-5 pb-4">
      <h3>Welcome to SIMON game simulator!</h3>
    </div>
    <div class="container col-12 col-sm-10 col-md-8 col-lg-6">
      <div class="circle" v-bind:class="{ circleShadows: activate }">
        <svg viewBox="0 0 42 42" class="donut">
          <circle class="donut-hole" cx="21" cy="21" r="15.91549430918954" fill="transparent"></circle>
          <circle class="donut-ring" cx="21" cy="21" r="15.91549430918954" fill="transparent" stroke="transparent" stroke-width="16"></circle>

          <circle id='blue' @mousedown="highlightButton($event)" @mouseup="removeHighlight($event)"
          class="donut-blue" :class="blueSequence" cx="21" cy="21" r="15.91549430918954"
          fill="none" stroke="#535f7f" stroke-width="10" stroke-dasharray="23 77" stroke-dashoffset="-1"></circle>
          <circle id='red' @mousedown="highlightButton($event)" @mouseup="removeHighlight($event)"
          class="donut-red" :class="redSequence" cx="21" cy="21" r="15.91549430918954"
          fill="none" stroke="#556180" stroke-width="10" stroke-dasharray="23 77" stroke-dashoffset="24"></circle>
          <circle id='green' @mousedown="highlightButton($event)" @mouseup="removeHighlight($event)"
          class="donut-green" :class="greenSequence" cx="21" cy="21" r="15.91549430918954"
          fill="none" stroke="#51628f" stroke-width="10" stroke-dasharray="23 77" stroke-dashoffset="49"></circle>
          <circle id='yellow' @mousedown="highlightButton($event)" @mouseup="removeHighlight($event)"
          class="donut-yellow" :class="yellowSequence" cx="21" cy="21" r="15.91549430918954"
          fill="none" stroke="#556180" stroke-width="10" stroke-dasharray="23 77" stroke-dashoffset="74"></circle>
          <g class="donut-text donutNameDeactivated" v-bind:class="{ donutNameActivated: activate, donutNameStart: start }">
            <text y="49%" transform="translate(0, 2)">
              <tspan @click="activate ? startGame() : ''" v-if="!start && !sequenceRunning && !playerRunning"  x="50%" text-anchor="middle" class="">START</tspan>
              <tspan v-else-if="start && sequenceRunning && !playerRunning"  x="50%" text-anchor="middle" class="">WAIT</tspan>  
              <tspan v-else x="50%" text-anchor="middle" class="">TRY</tspan>     
            </text>
          </g> 
          <g class="donut-level-text" v-bind:class="{ donutLevelTextActivated: activate }">          
            <text y="28%" transform="translate(0, 2)">
              <tspan x="50%" text-anchor="middle">Round</tspan>   
            </text>
          </g>           
          <g class="donut-indicator" v-bind:class="{ donutIndicatorActivated: activate }">          
            <text y="37%" transform="translate(0, 2)">
              <tspan x="50%" text-anchor="middle">{{ round }} | max {{ roundMax }}</tspan>   
            </text>
          </g>
          <g @click="activate ? (level1 = !level1, level2 = false, level3 = false) : ''"
          class="donut-level-number" v-bind:class="{ donutLevelActivated: level1 }">          
            <text y="60%" transform="translate(0, 2)">
              <tspan x="40%" text-anchor="middle">1</tspan>
            </text>
          </g>
          <g @click="activate ? (level2 = !level2, level1 = false, level3 = false) : ''"
          class="donut-level-number" v-bind:class="{ donutLevelActivated: level2 }">          
            <text y="60%" transform="translate(0, 2)">
              <tspan x="50%" text-anchor="middle">2</tspan>
            </text>
          </g>            
          <g @click="activate ? (level3 = !level3, level2 = false, level1 = false) : ''"
          class="donut-level-number" v-bind:class="{ donutLevelActivated: level3 }">          
            <text y="60%" transform="translate(0, 2)">
              <tspan x="60%" text-anchor="middle">3</tspan>
            </text>
          </g> 
          <g class="donut-level-text" v-bind:class="{ donutLevelTextActivated: activate }">          
            <text y="66%" transform="translate(0, 2)">
              <tspan x="50%" text-anchor="middle">Level</tspan>
            </text>
          </g> 
        </svg>
      </div>
      <transition name="slide-fade" out-in>
        <b-button v-if="!activate" @click="turnOnOff" class="mt-2" variant="success">
          On
        </b-button>
      </transition>
      <transition name="slide-fade" out-in>
        <b-button v-if="activate" @click="turnOnOff" class="mt-2" variant="danger">
          Off
        </b-button>
      </transition> 
      <div class="mt-5">
        <h3>How to play</h3>
        <ul class="text-left">
          <li>Turn it on</li>
          <li>Choose one of the difficulty levels</li>
          <li>Press START</li>
          <li>Repeat a sequence</li>
        </ul>
      </div>       
    </div>
    <br>
    <div class="mb-4">
      <h6>
        <span>
          <svg width="1em" height="1em" viewBox="0 0 16 16" class="bi bi-pencil icon" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" d="M11.293 1.293a1 1 0 0 1 1.414 0l2 2a1 1 0 0 1 0 1.414l-9 9a1 1 0 0 1-.39.242l-3 1a1 1 0 0 1-1.266-1.265l1-3a1 1 0 0 1 .242-.391l9-9zM12 2l2 2-9 9-3 1 1-3 9-9z"/>
            <path fill-rule="evenodd" d="M12.146 6.354l-2.5-2.5.708-.708 2.5 2.5-.707.708zM3 10v.5a.5.5 0 0 0 .5.5H4v.5a.5.5 0 0 0 .5.5H5v.5a.5.5 0 0 0 .5.5H6v-1.5a.5.5 0 0 0-.5-.5H5v-.5a.5.5 0 0 0-.5-.5H3z"/>
          </svg>          
        </span>
        Created by <a href="https://github.com/YegorKartcev">Yegor Kartcev</a>
      </h6>
    </div>
  </div>




</template>

<script>

var audio1 = new Audio(require('../sounds/1.mp3'));
var audio2 = new Audio(require('../sounds/2.mp3'));
var audio3 = new Audio(require('../sounds/3.mp3'));
var audio4 = new Audio(require('../sounds/4.mp3'));

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      message: "Works!",
      activate: false,
      start: false,
      sequenceRunning: false,
      playerRunning: false,
      level: false,
      level1: false,
      level2: false,
      level3: false,
      round: 0,
      roundMax: 0,
      redSequence: "",
      blueSequence: "",
      yellowSequence: "",
      greenSequence: "",
      currentSequence: [],
      playerSequence: [],
      lightFadingTime: 300
    }
  },
  methods: {
    highlightButton (evt) {
      let el = evt.target
      el.classList.add('donut-piece')

      if (this.playerRunning) {
        this.playerSequence.push(el.id)
      }
      if (el.id == 'red') {
        audio1.play()
      } else if (el.id == 'blue') {
        audio2.play()
      } else if (el.id == 'yellow') {
        audio3.play()
      } else {
        audio4.play()
      }  
    },
    removeHighlight (evt) {
      let el = evt.target
      el.classList.remove('donut-piece')
      // setTimeout(() => {
      //   el.classList.remove('donut-piece')
      // }, 300)
    },
    timeout(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    },
    turnOnOff() {
      this.activate = !this.activate
      this.round = 0
      this.roundMax = 0
    },
    startGame() {
      this.start = !this.start;
      this.currentSequence = []
      this.playerSequence = []
      this.sequenceRunning = false
      this.playerRunning = false

      if (this.start == true && this.playerRunning == false && this.sequenceRunning == false ) {
        this.sequenceRunning = true 
        this.timeout(1000).then(() => 
          this.blink().then(() => 
            this.playerResponse().then(() =>
              this.result())))
      }
    },
    async blink() {
      const colors = ["red", "blue", "yellow", "green"]
      const seqLength = this.round+1
      let delay=this.delay     
      for (let i = 0; i < seqLength; i++) {
        await this.timeout(delay*1000)
        let ranNumber = Math.floor(Math.random() * 4)
        let color = colors[ranNumber]
        if (color == "red") {
          audio1.play()
          this.redSequence = "donut-piece"
          await this.timeout(this.lightFadingTime)
          this.redSequence = "";
        } else if (color == "blue") {
          audio2.play()
          this.blueSequence = "donut-piece"
          await this.timeout(this.lightFadingTime)
          this.blueSequence = "";
        } else if (color == "yellow") {
          audio3.play()
          this.yellowSequence = "donut-piece"
          await this.timeout(this.lightFadingTime)
          this.yellowSequence = "";
        } else {
          this.greenSequence = "donut-piece"
          audio4.play()
          await this.timeout(this.lightFadingTime)
          this.greenSequence = "";
        }
        this.currentSequence.push(color)
      }
      return this.sequenceRunning = false 
    },
    async playerResponse() {
      this.playerRunning = true
      while (this.playerRunning) {
        await this.timeout(1000)
        if (this.playerSequence.length == this.currentSequence.length) {
            this.playerRunning = false
        }
      }
      return
    },
    result() {
      let error = false
      for (let i=0; i < this.currentSequence.length; i++) {
        if (this.playerSequence[i] != this.currentSequence[i]) {
          error = true;
          break
        }
      }
      if (!error) {
        this.round += 1
      } else {
        this.round = 0
      }
      if (this.round > this.roundMax) {
        this.roundMax = this.round
      }
      this.start = false;
      return
    }
  },
  computed: {
    delay: function () {
      if (this.level3) {
        return 0.4
      } else if (this.level2) {
        return 1
      } else {
        return 1.5
      }    
    }
  },
  watch: {
    activate: function () {
      if (!this.activate) {
        this.level = false;
        this.level1 = false;
        this.level2 = false;
        this.level3 = false
      } else {
        this.level1 = true;
      }
    },
   level1: function () { 
      if (!this.level2 && !this.level3 && this.activate) {
        this.level1 = true
      }
    },
    level2: function () {
      if (!this.level1 && !this.level3 && this.activate) {
        this.level2 = true
      }
    },
    level3: function () {
      if (!this.level1 && !this.level2 && this.activate) {
        this.level3 = true
      } 
    }   
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.circle {
  border-radius: 50%;
  background-color: rgb(36, 36, 36);
  box-shadow:
    -20px -20px 40px 20px rgb(27, 27, 27)  
}
.circleShadows {
  box-shadow:
    0px 0px 20px 10px #fff,
    -20px -20px 40px 20px rgb(27, 27, 27)
}

.donut {
  width: 90%;
  margin: 5px;
}

.donut-piece {
  opacity: 1!important;
  filter: saturate(300%) contrast(200%) !important;
  cursor: pointer;
}
.donut-blue {
  stroke: rgb(0, 153, 255);
  opacity: 0.5;
  cursor: pointer;
}
.donut-red {
  stroke: red;
  opacity: 0.5;
  cursor: pointer;
}
.donut-yellow {
  stroke: yellow;
  opacity: 0.5;
  cursor: pointer;
}
.donut-green {
  stroke: rgb(106, 255, 47);
  opacity: 0.5;
  cursor: pointer;
}

.donut-ring {
    stroke: rgb(36, 36, 36);
}

.donut-indicator {
  fill: rgb(94, 94, 94);
  transition: transform 0.5s;
  font-size: 0.2em;

}
.donutIndicatorActivated {
    fill: rgb(0, 255, 64);
    filter: saturate(300%) contrast(200%) drop-shadow(0px 0px 10px rgb(255, 255, 255));
    font-weight: bold;
    opacity: 1;
}

.donut-text {
  font-size: 0.25em;
  /* font-weight: bold; */
  fill: rgb(94, 94, 94);
  transition: transform 0.5s
}
.donut-text:hover {
    /* transform: scale(1.01, 1); */
    filter:  drop-shadow(0px 0px 1px rgb(255, 255, 255));
    cursor: pointer;
}
.donutNameActivated {
    fill: rgb(255, 255, 255);
}
.donutNameStart {
    font-weight: bold;
    fill: rgb(255, 255, 255);
    filter:  drop-shadow(0px 0px 1px rgb(255, 255, 255));
}

.donut-level-text {
  font-size: 0.12em;
  fill: rgb(94, 94, 94);
}
.donutLevelTextActivated {
  fill: rgb(255, 255, 255);
  filter:  drop-shadow(0px 0px 21px rgb(255, 255, 255));
}
.donut-level-number {
  font-size: 0.2em;
  fill: rgb(94, 94, 94);
}
.donut-level-number:hover {
    /* transform: scale(1.01, 1); */
    filter:  drop-shadow(0px 0px 1px rgb(255, 255, 255));
    cursor: pointer;
}
.donutLevelActivated {
  fill: rgb(255, 255, 255);
  font-weight: bold;
  filter:  drop-shadow(0px 0px 1px rgb(255, 255, 255));
}



.ribbon {
  position: absolute;
  right: 0px; top: 0px;
  z-index: 1;
  overflow: hidden;
  width: 150px; height: 150px;
  text-align: right;
}
.ribbon span {
  font-size: 16px;
  font-weight: bold;
  color: #FFF;
  /* text-transform: uppercase; */
  text-align: center;
  line-height: 40px;
  transform: rotate(45deg);
  -webkit-transform: rotate(45deg);
  width: 250px;
  display: block;
  background: #000000;
  background: linear-gradient(#000000 0%, #2e2e2e 100%);
  box-shadow: 0 3px 10px -5px rgba(0, 0, 0, 1);
  position: absolute;
  top: 25px; right: -80px;
}
.ribbon-link {
  color: #fff;
}



  .slide-fade-enter-active {
    transition: all 1.0s ease;
  }
  .slide-fade-leave-active {
    transition: all 1.0s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-fade-enter, .slide-fade-leave-to {
    transform: translateX(-100px);
    opacity: 0;
  }
  
</style>







