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
      // alert(this.message);
      this.messageList.push(this.message);
      axios
        .post("http://localhost:8080/api/v1/chat", {
          text: this.message,
          emotion: true,
          voicevox: true,
          return_wav : false,
        })
        .then((response: any) => {
          console.log("レスポンス", response.data);
          const data = JSON.parse(response.data);
          this.messageList.push(data.text);
          const emotionInput = document.getElementById("emotion") as HTMLInputElement;
          const emotionStrengthInput = document.getElementById("emotionStrength") as HTMLInputElement;
          console.log("emotion", data.emotion)
          console.log("strength", data.strength)
          emotionInput.value = data.emotion;
          emotionStrengthInput.value = data.strength;

          // const audioData = response.data.file;
          // const decodedAudioData = atob(audioData);
          // const arrayBuffer = new ArrayBuffer(decodedAudioData.length);
          // const view = new Uint8Array(arrayBuffer);
          // for (let i = 0; i < decodedAudioData.length; i++) {
          //   view[i] = decodedAudioData.charCodeAt(i);
          // }
          // const blob = new Blob([arrayBuffer], { type: "audio/wav" });
          // const audioUrl = URL.createObjectURL(blob);
          // const audio = new Audio(audioUrl);
          // audio.play()
          // console.log("audio : ", audioUrl);
        // .post("http://localhost:8080/api/v1/wav", {
        // }, {
        //   responseType: 'arraybuffer' // レスポンスのデータタイプを指定する
        // })
        //   .then((response: any) => {
        //     // レスポンスのデータをBlobオブジェクトに変換する
        //     const blob = new Blob([response.data], { type: 'audio/wav' });

        //     // BlobオブジェクトをURLに変換して、音声を再生する
        //     const url = URL.createObjectURL(blob);
        //     const audio = new Audio(url);
        //     audio.play();


          // axios
          //   .get(audioUrl, { responseType: "blob" })
          //   .then((response) => {
          //     const audio = new Audio();
          //     audio.src = URL.createObjectURL(response.data);
          //     const audioCtx = new AudioContext();
          //     const source = audioCtx.createMediaElementSource(audio);
          //     const analyser = audioCtx.createAnalyser();
          //     source.connect(analyser);
          //     analyser.connect(audioCtx.destination);
          //     analyser.fftSize = 256;
          //     const bufferLength = analyser.frequencyBinCount;
          //     const dataArray = new Uint8Array(bufferLength);

          //     let drawInterval: any;

          //     audio.addEventListener("play", () => {
          //       drawInterval = setInterval(() => {
          //         analyser.getByteFrequencyData(dataArray);
          //         const sum = dataArray.reduce((a, b) => a + b);
          //         const average = sum / bufferLength;
          //         console.log(average);
          //       }, 16);
          //     });
          //     audio.addEventListener("pause", () => {
          //       clearInterval(drawInterval);
          //     });
          //     audio.play();
          //   })
          //   .catch((error) => console.error(error));

          // const fs = require("fs");
          // const fileName = "../../Resources/Haru/sounds/audio.wav";
          // fs.writeFile(fileName, view, "binary", function (err: any) {
          //   if (err) throw err;
          //   console.log("ファイルが保存されました:", fileName);
          // });

          // auidioData を ローカルストレージに入れたい！

          // web audio apiを使って現在再生しているwavファイルの音量をaudio.plya()をしている間consoleに表示する
          // const audioCtx = new AudioContext()
          // const source = audioCtx.createMediaElementSource(audio)
          // const analyser = audioCtx.createAnalyser()
          // source.connect(analyser)
          // analyser.connect(audioCtx.destination)
          // analyser.fftSize = 256
          // const bufferLength = analyser.frequencyBinCount
          // const dataArray = new Uint8Array(bufferLength)

          // let drawInterval

          // audio.addEventListener('play', () => {
          //   drawInterval = setInterval(() => {
          //     analyser.getByteFrequencyData(dataArray)
          //     const sum = dataArray.reduce((a, b) => a + b)
          //     const average = sum / bufferLength
          //     console.log(average)
          //   }, 16)
          // })

          // audio.addEventListener('pause', () => {
          //   clearInterval(drawInterval)
          // })

          // audio.play()
          // const draw = () => {
          //   requestAnimationFrame(draw)
          //   analyser.getByteFrequencyData(dataArray)
          //   const sum = dataArray.reduce((a, b) => a + b)
          //   const average = sum / bufferLength
          //   console.log(average)
          // }
          // draw()
          // audio.play()

          // const fs = require('fs')
          // // 書き出すファイル名を指定する
          // const fileName = 'audio.wav'

          // バイナリデータを書き出す
          // fs.writeFile(fileName, view, 'binary', function (err: any) {
          //   if (err) throw err
          //   console.log('ファイルが保存されました:', fileName)
          // })

          // レスポンスJSONデータを取得する
          // console.log("audioUrl : ", audioUrl);
          // console.log("レスポンス", response.data.data);

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
