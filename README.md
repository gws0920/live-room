# 在线房间
用于在线的语音/视频通话、屏幕共享、聊天等。

## Feature
- [] 基于[SimpleWebRTC](https://github.com/simplewebrtc/SimpleWebRTC)实现主要功能
- [] 用户: 不需要登录，用户信息存储到浏览器本地。[multiavatar](https://multiavatar.com/)生成随机头像([github](https://github.com/multiavatar/Multiavatar))。自己写个随机名字生成器。
- [] 页面（房主）：可管理房间
- [] 页面（观众）：加入房间
- [] 通用功能：实时聊天、控制他人的音量
- [] 基于[Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)的语音转文字
```js
var recognition = new webkitSpeechRecognition();
    recognition.lang = 'zh-CN'; // 设置语言为中文

    document.getElementById('startButton').addEventListener('click', function() {
      // 获取用户媒体流（通常是麦克风）
      navigator.mediaDevices.getUserMedia({ audio: true })
      .then(function(stream) {
        // 将音频流传递给语音识别对象
        recognition.stream = stream;

        // 启动语音识别
        recognition.start();

        // 当识别结束时，停止音频流
        recognition.onend = function() {
          stream.getTracks().forEach(track => track.stop());
        };

        // 当识别到结果时，将结果显示在页面上
        recognition.onresult = function(event) {
          var result = event.results[event.results.length - 1][0].transcript;
          document.getElementById('result').textContent = result;
        };

        recognition.onerror = function(event) {
          console.error('语音识别出错', event.error);
        };
      })
      .catch(function(err) {
        console.error('获取媒体流失败', err);
      });
    });
```