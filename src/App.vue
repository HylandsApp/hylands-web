<template>

  <!-- header -->
  <header>

    <h1 style="margin: 0; padding: 0;">Hylands <span style="font-size: 0.875rem;">A NeoMUD</span></h1>

  </header>

  <!-- xterm --> 
  <FullscreenXterm />

  <!-- footer -->
  <footer>

    &copy; 2025 Hylands.app

  </footer>

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