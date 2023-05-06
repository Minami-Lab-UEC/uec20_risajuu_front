<template>
  <div id="app">
    <div v-for="i in messageList" :key="i">
      <div>{{ i }}</div>
    </div>
    <input type="text" v-model="message" />
    <button id="test" @click="sendMessage">送信</button>

    <div>
      <input id="emotion" type="text" value="Joy" />
      <input id="emotionStrength" type="text" value="0.65" />
      <button id="emotionButton"></button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  data() {
    return {
      message: "",
      messageList: [""],
    };
  },
  methods: {
    sendMessage() {
      alert(this.message);
      this.messageList.push(this.message);
      axios
        .post("http://localhost:8080/api/v1/chat", {
          text: this.message,
          emotion: true,
        })
        .then((response: any) => {
          console.log(response);
          this.messageList.push(response.data.response);
          document.getElementById("emotionButton")?.click(); // 疑似的に隠されている感情を変更するボタンを押す
        })
        .catch((error: any) => {
          console.log(error);
        });
    },
  },
});
</script>

<style>
body {
  display: flex;
  flex-direction: column-reverse;
  align-items: center;
  justify-content: center;
}
</style>
