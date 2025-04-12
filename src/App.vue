<template>

  <!-- header -->
  <header>

    <h1 class="adventurer" style="margin: 0; padding: 0;">Hylands <span class="adventurer" style="font-size: 0.875rem;">A NeoMUD</span></h1>

  </header>

  <!-- xterm --> 
  <FullscreenXterm />

  <!-- footer -->
  <footer>

    <span class="adventurer" style="font-size: 0.75rem;">&copy; 2025 Hylands.app</span>

  </footer>

  <!-- grid -->
  <!-- <div class="grid-container">
    <div class="grid">
      <div class="grid-inner">
        <div class="hori">
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
        </div>
        <div class="vert">
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
          <div class="line"></div>
        </div>
      </div>
    </div>
  </div> -->

  <!-- dev panel 1 -->
  <div class="dev-panel dev-panel--1">

    <!-- top -->
    <div class="dev-panel__top">

      <span>Dev Panel 1</span>

    </div>

    <!-- middle-->
    <div class="dev-panel__middle">

      <div class="dev-panel__btn-group">

        <input ref="testTextInput" type="text" v-model="testTextModel" placeholder="frontend gen text">        

        <button ref="generateBtn" @click="testInputSubmit">gen</button>

      </div>

      <span>frontend gen: {{ frontendGenStatus }}</span>           

      <audio ref="audioElement" id="audio-player" autoplay></audio>

    </div>
    
    <!-- bottom -->
    <div class="dev-panel__bottom">

      <!-- placeholder -->

    </div>

  </div>

</template>

<style lang="scss">
 
  // header
  header {

  }

  // footer
  footer {
    text-align: center;
  }

  // dev panel 1
  .dev-panel {
    position: fixed;
    
    // top
    &__top {

    }
    
    // middle
    &__middle {
      display: flex;
      flex-direction: column;
    }

    // bottom
    &__bottom {

    }

    // btn group
    &__btn-group {
      display: flex;
    }

    // mods
    &--1 {
      top: 1rem;
      right: 1rem;
    }
  }

  // $speed: 5s;
  // $increase-vert: $speed/10;
  // $scanlines: 1px;

  // @keyframes blur-n-hue {
  //   0% {filter: blur(10px) hue-rotate(-15deg);}
  //   50% {filter: blur(20px) hue-rotate(0deg);}
  //   100% {filter: blur(10px) hue-rotate(-15deg);}
  // }
  // @keyframes move {
  //   from {top: 0%; opacity: 0.8;}
  //   to {top: 100%; opacity: 1;}
  // }
  // @keyframes angle {
  //   0% {transform: rotateX(58deg);}
  //   50% {transform: rotateX(60deg);}
  //   100% {transform: rotateX(58deg);}
  // }
  // @keyframes bg {
  //   0% {background-color: rgba(46, 46, 46, 0);}
  //   49% {background-color: rgba(46, 46, 46, 0);}
  //   50.5% {background-color: rgba(155, 155, 206, .4);}
  //   51% {background-color: rgba(46, 46, 46, 0);}
  //   51.5% {background-color: rgba(155, 155, 206, .3);}
  //   57% {background-color: rgba(46, 46, 46, 0);}
  //   100% {background-color: rgba(46, 46, 46, 0);}
  // }

  // .grid-container {
  //   position: fixed;
  //   bottom: 0;
  //   left: 0;
  //   right: 0;
  //   width: 100%;
  //   height: 75%;
  //   // opacity: 0.1;
  // }

  // .grid {
  //   perspective: 200px;
  //   width: 100%;
  //   height: 100%;
  //   position: relative;

  //   .grid-inner {
  //     position: absolute;
  //     top:0;
  //     left:0;
  //     right:0;
  //     bottom:0;
  //     width:100%;
  //     height:100%;
  //     transform: rotateX(60deg);
  //     //animation: angle 5s infinite ease-in-out;
  //   }

  // }
  
  // .line {
  //   display: block;
  //   position: absolute;
  //   width: 0px;
  //   top: 0;
  //   left: 0;
  //   right: 0;
  //   bottom: 0;
  //   border: 1px solid rgba(0,225,0,1);
  //   box-sizing: border-box;
  //   height: 100%;
  //   background: rgba(0,225,0,1);
  //   box-shadow: 0px 0px 6px 2px rgba(0,225,0,1);
  // }

  // .hori, .vert {
  //   position: absolute;
  //   top: 0;
  //   left: 0;
  //   right: 0;
  //   bottom: 0;
  //   width: 100vw;
  //   height: 100vh;
  // }

  // .hori {
  //   .line {
  //     &:nth-child(1) {left: -40%;}
  //     &:nth-child(2) {left: -20%;}
  //     &:nth-child(3) {left: 0%;}
  //     &:nth-child(4) {left: 20%;}
  //     &:nth-child(5) {left: 40%;}
  //     &:nth-child(6) {left: 60%;}
  //     &:nth-child(7) {left: 80%;}
  //     &:nth-child(8) {left: 100%;}
  //     &:nth-child(9) {left: 120%;}
  //     &:nth-child(10) {left: 140%;}
  //   }
  // }

  // .vert {
  //   .line {
  //     animation: move $speed infinite linear;
  //     width: 200%;
  //     left: -50%;
  //     height: 0px;
  //     &:nth-child(1) {animation: none; opacity: 0.7;}
  //     &:nth-child(2) {animation-delay: $increase-vert;}
  //     &:nth-child(3) {animation-delay: $increase-vert*2;}
  //     &:nth-child(4) {animation-delay: $increase-vert*3;}
  //     &:nth-child(5) {animation-delay: $increase-vert*4;}
  //     &:nth-child(6) {animation-delay: $increase-vert*5;}
  //     &:nth-child(7) {animation-delay: $increase-vert*6;}
  //     &:nth-child(8) {animation-delay: $increase-vert*7;}
  //     &:nth-child(9) {animation-delay: $increase-vert*8;}
  //     &:nth-child(10) {animation-delay: $increase-vert*9;}
  //   }
  // }  

</style>

<script setup lang="ts">

  // imports
  import { ref, watch } from 'vue'
  import FullscreenXterm from './components/FullscreenXterm.vue'
  import { KokoroTTS } from 'kokoro-js'
  import { encode } from 'wav-encoder'

  // references
  const generateBtn = ref<HTMLButtonElement | null>(null)
  const testTextInput = ref<HTMLInputElement | null>(null)
  const testTextModel = ref<string>('')
  const audioElement = ref<HTMLAudioElement | null>(null)
  const frontendGenStatus = ref<string>('idle')
  
  // methods
  const testTextInputEvent = () => {
    // console.log('testTextInputEvent')
    // console.log(testTextModel.value)
  }

  const testInputSubmit = async () => {

    // console.log('testInputSubmit')
    // console.log(testTextModel.value)

    // if no text, return
    if (testTextModel.value === '') {
      return
    }

    // try
    try {

      // log
      console.log('creating tts object')
      frontendGenStatus.value = 'creating tts object'
      
      // create the tts object
      const tts = await KokoroTTS.from_pretrained('onnx-community/Kokoro-82M-ONNX', { dtype: "q8" })

      // log
      console.log('generating audio')
      frontendGenStatus.value = 'generating audio'

      // generate audio
      const audio = await tts.generate(testTextModel.value, { voice: 'af_bella' })

      // 
      if( !audio || !audio.audio ){

        // return error
        return console.error('threre was an error trying to generate audio')

      }

      // log
      console.log('combining audio data')
      frontendGenStatus.value = 'combining audio data'

      // 
      const audioData = {
        sampleRate: audio.sampling_rate,
        channelData: [audio.audio],
      }

      // 
      if ( !audioData ){

        // return error
        return console.error('threre was an error trying to generate audio')

      }

      // log
      console.log('encoding audio data')
      frontendGenStatus.value = 'encoding audio data'

      // encode the audio data
      const encodedAudio = await encode(audioData)

      // if no encoded audio, return error
      if ( !encodedAudio ){
        return console.error('threre was an error trying to encode audio')
      }

      // log
      console.log('creating blob')
      frontendGenStatus.value = 'creating blob'

      // create blob from encoded audio
      const blob = new Blob([encodedAudio], { type: 'audio/wav' })
      
      // log
      console.log('creating object url')
      frontendGenStatus.value = 'creating object url'

      // create object url
      const audioUrl = URL.createObjectURL(blob)
      
      // log
      console.log('setting audio source and playing')
      frontendGenStatus.value = 'setting audio source and playing'

      // set audio source and play
      if (audioElement.value) {
        audioElement.value.src = audioUrl
        audioElement.value.play()
      }

      // log
      console.log('audio playing')
      frontendGenStatus.value = 'idle'
      
    } catch (error) {

      // log error
      console.error('threre was an error trying to [put info here] ', error)
      frontendGenStatus.value = 'error'

    }

  }

  // watch
  watch(testTextModel, (newVal) => {
    // console.log('testTextModel changed')
    // console.log(newVal)
  })

</script>