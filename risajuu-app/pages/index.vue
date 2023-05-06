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
          voicevox: true,
        },{
          // responseType: 'arraybuffer' // レスポンスのデータタイプを指定する
        })
        .then((response: any) => {
          console.log("レスポンス", response.data);
          //this.messageList.push(response.data.response);
          // const jsonBlob = new Blob([response.data[0]], { type: 'application/json' });
          // const blob = new Blob([response.data], { type: 'audio/wav' });

          // // BlobオブジェクトをURLに変換して、音声を再生する
          // const url = URL.createObjectURL(blob);
          // const audio = new Audio(url);
          // audio.play();

          const audioData = response.data.file
          const decodedAudioData = atob(audioData)
          const arrayBuffer = new ArrayBuffer(decodedAudioData.length)
          const view = new Uint8Array(arrayBuffer)
          for (let i = 0; i < decodedAudioData.length; i++) {
            view[i] = decodedAudioData.charCodeAt(i)
          }
          const blob = new Blob([arrayBuffer], { type: 'audio/wav' })
          const audioUrl = URL.createObjectURL(blob)
          const audio = new Audio(audioUrl)
          audio.play()

          // レスポンスJSONデータを取得する
          // const responseData = response.headers;
          // console.log(responseData);
          console.log("レスポンス", response.data.data);
  
          // document.getElementById("emotionButton")?.click(); // 疑似的に隠されている感情を変更するボタンを押す
        })
        // .then(response => response.formData())
        // .then(formData => {
        //   const jsonFile = formData.get('json_file');
        //   const wavFile = formData.get('wav_file');
        //   document.getElementById("emotionButton")?.click(); // 疑似的に隠されている感情を変更するボタンを押す

        //   console.log(jsonFile);
        //   console.log(wavFile);
        // });
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
