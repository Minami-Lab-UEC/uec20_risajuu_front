<template>
  <div id="app">
    <div v-for="i in messageList" :key="i">
      <div>{{ i }}</div>
    </div>
    <input type="text" v-model="message" />
    <button id="test" @click="sendMessage">送信</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios'

export default defineComponent({
  data() {
    return {
      message: '',
      messageList: [''],
    }
  },
  methods: {
    sendMessage() {
      alert(this.message)
      this.messageList.push(this.message);
      axios.post('http://localhost:8080/api/v1/chat', {
        text: this.message,
        // emotionAnalysis: true
      })
      .then((response: any) => {
        console.log(response)
        this.messageList.push(response.data.response);
      })
      .catch((error: any) => {
        console.log(error)
      })
    }
  }
})
</script>

<style>
body {
  display: flex;
  flex-direction: column-reverse;
  align-items: center;
  justify-content: center;
}
</style>
