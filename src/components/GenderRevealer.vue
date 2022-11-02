<template>
  

 

    <div id="balloon-container">
      <div class="text-wrapper">
        
        <div class="text">GENDER REVEAL</div>

        <!-- default -->
        <span v-if="!isDecided && !isComingSoon">
          <div class="text">BOY</div>
          <div class="text">or</div>
          <div class="text">GIRL</div>
          <div class="text">Return here at</div>
          <div class="text">Friday</div>
          <div class="text">July 29th 6:00pm</div>
        </span>

        <!-- coming soon -->
        <span v-if="!isDecided && isComingSoon">
           <div class="text">BOY</div>
          <div class="text">or</div>
          <div class="text">GIRL</div>
          <div class="text">ALMOST THERE!</div>
        </span>

        <!-- decided! -->
        <span v-if="isDecided">
          <div class="text">IT'S</div>
          <div class="text">A</div>
          <div class="text">{{g + i + r + l}}</div>
        </span>
        
        

        <!-- need to account for daylight savings ...  -->
        <Countdown class="text" deadline="July 29, 2022 05:00 PM EST"></Countdown>

            <div class="music-player">
    <audio
      ref="audio"
      
      preload
      loop
      autoplay
      id="audio"
      
      :muted="isMuted"
    >
    <source src="@/assets/music/music.mp3" type="audio/mp3">
    </audio>
    <div @click="toggleSound()" class="toggle-sound"><div style="padding-top:20px">click me for music</div></div>
  </div>
    </div>
  </div>
</template>

<script>
import Countdown from 'vuejs-countdown'

export default {
  name: "GenderRevealer",
  components: {Countdown},
  data(){
    return {
      isblue: true,
      isDecided: false,
      isComingSoon: false,
      targetDate: new Date('July 29, 2022 05:00 PM EST'),
      comingSoonDate: new Date('July 29, 2022 04:30 PM EST'),
      isMuted: true
    }
  },
  computed:{
    g(){
      return 'B'
    },
    i(){
      return 'O'
    },
    r(){
      return 'Y'
    },
    l(){
      return ''
    }
  },
  methods:{
     toggleSound(){
      this.isMuted = !this.isMuted;
      if (this.isMuted){
        document.getElementById('audio').muted=true;
      }else {
        document.getElementById('audio').muted=false;
        document.getElementById('audio').play();
      }
    },
    updateBalloonColors(){
      let balls = document.getElementsByClassName('balloon');
      for (var x = 0; x < balls.length; x++){
        balls[x].style.cssText = document.getElementsByClassName('balloon')[x].style.cssText.replace(document.getElementsByClassName('balloon')[x].style.cssText.split(';')[0], this.getNewBackgroundColor())
        balls[x].style.cssText = document.getElementsByClassName('balloon')[x].style.cssText.replace(document.getElementsByClassName('balloon')[x].style.cssText.split(';')[1], " " + this.getNewColor())
        balls[x].style.cssText = document.getElementsByClassName('balloon')[x].style.cssText.replace(document.getElementsByClassName('balloon')[x].style.cssText.split(';')[2], " " + this.getNewBoxShadow())
      }
    },
    flopBackgroundColor(){
      if (document.getElementsByTagName('body')[0].classList.contains('blue')){
        //flip to pink
        document.getElementsByTagName('body')[0].classList.add('pink');
        document.getElementsByTagName('body')[0].classList.remove('blue');
        this.isblue = false;
      }else{
        document.getElementsByTagName('body')[0].classList.remove('pink');
        document.getElementsByTagName('body')[0].classList.add('blue');
        this.isblue = true;
      }
      this.updateBalloonColors();
    },
    createBalloons(num) {
      var balloonContainer = document.getElementById("balloon-container")
        for (var i = num; i > 0; i--) {
        var balloon = document.createElement("div");
        balloon.className = "balloon";
        balloon.style.cssText = self.isblue ? this.getRandomPinkStyles() : this.getRandomStyles();           
        balloonContainer.append(balloon);
    }
},
getNewBackgroundColor(){
  if (this.isblue){
    return `background-color: rgba(${this.random(20) + 230},${110},${211},0.7);`
  }else{
    return `background-color: rgba(${66},${188},${this.random(20) + 230},0.7);`
  }
},
getNewColor(){
  if (this.isblue){
    return `color: rgba(${this.random(20) + 230},${110},${211},0.7);`
  }else{
    return `color: rgba(${66},${188},${this.random(20) + 230},0.7);`
  }
},
getNewBoxShadow(){
  if (this.isblue){
    return `box-shadow: inset -7px -3px 10px rgba(${50},${50},${50},0.7);`
  }else{
    return `box-shadow: inset -7px -3px 10px rgba(${50},${50},${50},0.7);`
  }
},
getRandomStyles() {
  var r = 66;
  var g = 188;
  var b = this.random(20)+230;
  var mt = this.random(200) - 200;
  var ml = this.random(50);
  var dur = this.random(5)+20;
  return `
  background-color: rgba(${r},${g},${b},0.7);
  color: rgba(${r},${g},${b},0.7); 
  box-shadow: inset -7px -3px 10px rgba(${50},${50},${50},0.7);
  margin: ${mt}px 0 0 ${ml}px;
  animation: float ${dur}s ease-in infinite;
  transition: background-color 1s ease, color 1s ease;
  `
},
getRandomPinkStyles(){
  var r = this.random(20) + 230;
  var g = 110;
  var b = 211;
  var mt = this.random(200) - 200;
  var ml = this.random(50);
  var dur = this.random(5)+20;
  return `
  background-color: rgba(${r},${g},${b},0.7);
  color: rgba(${r},${g},${b},0.7); 
  box-shadow: inset -7px -3px 10px rgba(${50},${50},${50},0.7);
  margin: ${mt}px 0 0 ${ml}px;
  animation: float ${dur}s ease-in infinite;
  transition: background-color 1s ease, color 1s ease;
  `
},
random(num){
  return Math.floor(Math.random()*num)
}




  },
  created(){
    let self = this;
    setInterval(() => {
      self.flopBackgroundColor();
    },2000)

  },
  mounted(){
    let self = this;
    self.createBalloons(100);
    document.body.scrollTop = 0;
    document.documentElement.scrollTop = 0;
    setInterval(() => {
      let now = new Date(Date.now());

      //check isComingSoon
      if (now > self.targetDate){
        self.isDecided = true;
      }
      else if (now > self.comingSoonDate){
        self.isComingSoon = true;
        // console.log('coming soon! ');
      }else{
        // console.log('Still waiting');
      }
    }, 1000)
  }
}
</script>

<style>
.blue{
  
  background-color: lightblue;
}

.pink{
  background-color: pink;
}




body {
  margin: 0;
  transition: background-color 1s ease;
}

#balloon-container {
  height: 100vh;
  padding: 1em;
  margin-top: -70px;
  box-sizing: border-box;
  display: flex;
  flex-wrap: wrap;
  overflow: hidden;
}

.balloon {
  height: 125px;
  width: 105px;
  border-radius: 75% 75% 70% 70%;
  position: relative;
}

.balloon:before {
  content: "";
  height: 75px;
  width: 1px;
  padding: 1px;
  background-color: #FDFD96;
  display: block;
  position: absolute;
  top: 125px;
  left: 0;
  right: 0;
  margin: auto;
}

.balloon:after {
    content: "▲";
    text-align: center;
    display: block;
    position: absolute;
    color: inherit;
    top: 120px;
    left: 0;
    right: 0;
    margin: auto;
}

@keyframes float {
  from {transform: translateY(100vh);
  opacity: 1;}
  to {transform: translateY(-300vh);
  opacity: 0;}
}

.text-wrapper{
  display:flex;
  justify-content:center;
  width: 100%;
  flex-direction: column;
  font-weight: bold;
}

.text{
  padding-top:20px;
  font-size:30px;
}

.vuejs-countdown{
  margin-top:20px !important;
}

.toggle-sound{
  height: 75px;
  background-color: #d3d3d369;
  width: 200px;
  margin:auto;
  border-radius: 10px;
  box-shadow: 16px 9px 5px 1px rgba(0,0,0,0.49);
-webkit-box-shadow: 16px 9px 5px 1px rgba(0,0,0,0.49);
-moz-box-shadow: 16px 9px 5px 1px rgba(0,0,0,0.49);
  text-align:center;
  z-index: 100;
  cursor: pointer;
}
</style>