<script setup lang="ts">
  import { ref, onMounted, Ref } from 'vue'

  const transcript: Ref<string> = ref('')
  const isRecording: Ref<boolean> = ref(false)

  const Recognition: any = window.SpeechRecognition || window.webkitSpeechRecognition;

  const sr = new Recognition()

  onMounted(() => {
	  sr.continuous = true;
	  sr.interimResults = true;
    
    sr.onstart = () => {
      isRecording.value = true
    }
    
    sr.onend = () => {
      isRecording.value = false
    }

    sr.onresult = (evt: { results: any[] }) => {
      console.log(evt)

      const t = Array.from(evt.results)
        .map((result: any[]) => result[0])
        .map((result: { transcript: any }) => result.transcript)
        .join('')

      transcript.value = t
    }
  })

  const toggleMic = () => {
    if (isRecording.value) {
      sr.stop()
    } else {
      sr.start()
    } 
  }

</script>

<template>
  <div class="app">
    <button :class="`mic`" @click="toggleMic">Record</button>
    <div class="transcript" v-text="transcript"></div>
  </div>

</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.mic {
  margin: 25px;
  padding: 15px;
  font-size: 2.5rem;
  background: rgba(0, 0, 0, 0.05);
  border: none;
  cursor: pointer;
}

.mic:hover {
  background: rgba(0, 0, 0, 0.1);
}

.transcript {
  font-size: 2.5rem;
}
</style>
